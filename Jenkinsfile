pipeline {
     agent any
     options {
         timestamps()
         ansiColor('xterm')
         disableConcurrentBuilds()
     }
     environment {
         GIT_COMMIT_SHORT = sh(
                 script: "printf \$(git rev-parse --short=8 ${GIT_COMMIT})",
                 returnStdout: true
         )

         GIT_REPO_NAME = sh(
             script: 'echo ${GIT_URL} | awk -F / \'{print $NF}\'|sed \'s@.git@@g\'',
             returnStdout: true).trim()

         K8S_NAMESPACE = "glzh-dev"

         SERVICE_NAME = 'gl-docs-spec'

         DOCKER_FILE = 'Dockerfile'

         IMG_PREFIX = "swr.cn-north-4.myhuaweicloud.com/glzh-dev"
     }

     stages {
         stage('Prepare Build') {
             steps {
                 script {
                     if (env.GIT_URL.contains("glzh"))  {
                         env.GIT_GROUP_NAME = sh(
                             script: 'echo ${GIT_URL}|awk -F / \'{print $5}\'',
                             returnStdout: true).trim()
                     } else {
                         env.GIT_GROUP_NAME = sh(
                             script: 'echo ${GIT_URL}|awk -F / \'{print $4}\'',
                             returnStdout: true).trim()
                     }
                 }
                 echo "${GIT_GROUP_NAME}"
             }
         }
         stage('Create Build Workspace') {
             steps {
                 sh """
                     mkdir -p buildImage
                     cp ${DOCKER_FILE} buildImage
                     cp -r ${BUILD_PATH} buildImage
                     rm -rf buildImage/.git|true
                     rm -rf buildImage/nginx
                     rm -f  buildImage/Jenkinsfile
                     cp -r nginx buildImage
                 """
              }
         }
         stage('Build Image') {
             steps {
                 sh """
                     cd buildImage
                     docker build -t ${IMG_PREFIX}/${SERVICE_NAME}:${GIT_COMMIT_SHORT} -f ${DOCKER_FILE} .
                     docker tag ${IMG_PREFIX}/${SERVICE_NAME}:${GIT_COMMIT_SHORT} ${IMG_PREFIX}/${SERVICE_NAME}:${IMAGE_TAG}
                 """
              }
         }
         stage('Push Image') {
             steps {
                 sh """
                     docker push ${IMG_PREFIX}/${SERVICE_NAME}:${GIT_COMMIT_SHORT}
                     docker push ${IMG_PREFIX}/${SERVICE_NAME}:${IMAGE_TAG}
                 """
              }
         }
         stage('Get Deploy repo') {
             steps {
                 script {
                     sshagent(['ci-bot']) {
                     sh """
                         mkdir -p deploy && cd deploy
                         git clone ssh://git@dev-git.gaolvzongheng.com:36622/glzh/ops/charts.git
                         git clone --branch ci-bot ssh://git@dev-git.gaolvzongheng.com:36622/glzh/ops/gitops.git
                         git config user.name 'ci-bot'
                         git config user.email 'ci-bot@gaolvgo.com'
                     """
                     }
                 }
             }
         }
         stage('generator yaml') {
             agent {
                 docker {
                     reuseNode true
                     image "swr.cn-north-4.myhuaweicloud.com/glzh-library/deploy:v1.0.0"
                     args """
                         -e HOME=/kube
                         -v $HOME/.kube/huawei-dev-config:/kube/.kube/config
                     """
                 }
             }
             steps {
                 script {
                     sh """#!/bin/bash
                         set -x
                         cd deploy
                         mkdir -p gitops/${GIT_GROUP_NAME}/${SERVICE_NAME}/dev
                         if ! [ -f gitops/${GIT_GROUP_NAME}/${SERVICE_NAME}/dev/istio.yaml ];then
                            sed -e "s@{{ .Release.Name }}@${SERVICE_NAME}@g" \
                                 -e "s@{{ .Values.namespace }}@${K8S_NAMESPACE}@g" charts/istio-template.yaml > gitops/${GIT_GROUP_NAME}/${SERVICE_NAME}/dev/istio.yaml
                         fi
                         helm template --set image=${IMG_PREFIX}/${SERVICE_NAME}:${GIT_COMMIT_SHORT} \
                           ${SERVICE_NAME} charts/nodejs > gitops/${GIT_GROUP_NAME}/${SERVICE_NAME}/dev/app.yaml
                     """
                 }
             }
         }
         stage ('Push deploy yaml') {
             steps {
                 script {
                     sshagent(['ci-bot']) {
                     sh """#!/bin/bash
                         set -x
                         cd deploy/gitops
                         git config user.name 'ci-bot'
                         git config user.email 'ci-bot@gaolvgo.com'
                         git status -s
                         git add .
                         git diff --cached
                     """
                     sh """#!/bin/bash
                         set -x
                         if git diff --cached;then
                           cd deploy/gitops
                           git pull --rebase
                           git commit -m "[JENKINS-CI] Add ${GIT_GROUP_NAME}/${SERVICE_NAME}/dev at ${BUILD_DISPLAY_NAME} config"
                           git push origin ci-bot
                         fi
                     """
                     }
                 }
             }
         }
         stage ('Deploy dev') {
             agent {
                 docker {
                     reuseNode true
                     image "swr.cn-north-4.myhuaweicloud.com/glzh-library/deploy:v1.0.0"
                     args """
                         -e HOME=/kube
                         -v $HOME/.kube/huawei-dev-config:/kube/.kube/config
                     """
                 }
             }
             steps {
                 sh """
                     kubectl apply -f deploy/gitops/${GIT_GROUP_NAME}/${SERVICE_NAME}/dev
                 """
              }
         }
     }
     post {
         always {
             sh """
                 rm -rf buildImage|true
                 rm -rf deploy|true
                 rm -rf dist|true
             """
         }
     }
}
