pipeline {
  agent any
  stages {
    stage('检出') {
      steps {
        checkout([$class: 'GitSCM',
        branches: [[name: GIT_BUILD_REF]],
        userRemoteConfigs: [[
          url: GIT_REPO_URL,
          credentialsId: CREDENTIALS_ID
        ]]])
      }
    }
    stage('构建镜像') {
      steps {
        sh 'docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION} .'
        sh 'docker images'
      }
    }
    stage('推送制品到镜像仓库') {
      steps {
        useCustomStepPlugin(key: 'SYSTEM:artifact_docker_push', version: 'latest', params: [repo:'main',image:'${CODING_DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}',properties:'[]'])
      }
    }
    stage('部署到远端服务') {
      steps {
        script {
          def remoteConfig = [:]
          remoteConfig.name = "my-remote-server"
          remoteConfig.host = "${REMOTE_HOST}"
          remoteConfig.port = "${REMOTE_SSH_PORT}".toInteger()
          remoteConfig.allowAnyHosts = true

          withCredentials([
            usernamePassword(
              // REMOTE_CRED 是一个环境变量，环境变量的值就是凭据的 ID
              // 当然也可以直接写固定值，credentialsId: "xxxx-xxxx-xxxxx"
              credentialsId: "${REMOTE_CRED}",
              passwordVariable: 'password',
              usernameVariable: 'userName'
            ),
            usernamePassword(
              credentialsId: "${CODING_ARTIFACTS_CREDENTIALS_ID}",
              usernameVariable: 'CODING_DOCKER_REG_USERNAME',
              passwordVariable: 'CODING_DOCKER_REG_PASSWORD'
            )
          ]) {
            // SSH 登录用户名
            remoteConfig.user = "${REMOTE_USER_NAME}"
            // SSH 密码
            remoteConfig.password = password

            // 请确保远端环境中有 Docker 环境
            sshCommand(
              remote: remoteConfig,
              command: "docker login -u ${CODING_DOCKER_REG_USERNAME} -p ${CODING_DOCKER_REG_PASSWORD} ${CODING_DOCKER_REG_HOST}",
              sudo: true,
            )

            sshCommand(
              remote: remoteConfig,
              command: "docker rm -f ${DOCKER_IMAGE_NAME} | true",
              sudo: true,
            )

            // DOCKER_IMAGE_VERSION 中涉及到 GIT_LOCAL_BRANCH / GIT_TAG / GIT_COMMIT 的环境变量的使用
            // 需要在本地完成拼接后，再传入到远端服务器中使用
            DOCKER_IMAGE_URL = sh(
              script: "echo ${CODING_DOCKER_REG_HOST}/${CODING_DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}",
              returnStdout: true
            )

            sshCommand(
              remote: remoteConfig,
              command: "docker run -d -p 8080:8080 --name ${DOCKER_IMAGE_NAME} ${DOCKER_IMAGE_URL}",
              sudo: true,
            )

            echo "部署成功，请到 http://${REMOTE_HOST}:8080 预览效果"
          }
        }

      }
    }
  }
  environment {
    CODING_DOCKER_REG_HOST = "${CCI_CURRENT_TEAM}-docker.pkg.${CCI_CURRENT_DOMAIN}"
    CODING_DOCKER_IMAGE_NAME = "${PROJECT_NAME.toLowerCase()}/${DOCKER_REPO_NAME}/${DOCKER_IMAGE_NAME}"
  }
}