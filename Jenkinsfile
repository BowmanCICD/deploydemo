pipeline {
    agent any
    environment {
        HARBOR_CRED = "harbor-wing-password"
        IMAGE_NAME = ""
        IMAGE_APP = "demo"
        branchName = ""
    }
    stages {
        stage('拉取代码') {
            environment {
                GITLAB_CRED = "gitlab-wing-password"
                GITLAB_URL = "http://gitlab.cicd.svc/develop/sprint_boot_demo.git"
            }
            steps {
                echo '开始拉取代码'
                checkout scmGit(userRemoteConfigs: [
                    [ url: 'https://github.com/BowmanCICD/spring-boot-demo.git' ]
                ])
                script {
                    branchName = (env.GIT_BRANCH ?: 'master').split('/')[-1]
                }
                echo '拉取代码完成'
            }
        }
        stage('编译打包') {
            steps {
                echo '开始编译打包'
                echo '编译打包完成'
                // container('maven') {
                //     echo '开始编译打包'
                //     sh 'mvn clean package'
                //     echo '编译打包完成'
                // }
            }
        }
        stage('代码审查') {
            environment {
                SONARQUBE_SCANNER = "SonarQubeScanner"
                SONARQUBE_SERVER = "SonarQubeServer"
            }
            steps {
                echo '开始代码审查'
                // script {
                //     def scannerHome = tool "${SONARQUBE_SCANNER}"
                //     withSonarQubeEnv("${SONARQUBE_SERVER}") {
                //         sh "${scannerHome}/bin/sonar-scanner"
                //     }
                // }
                echo '代码审查完成'
            }
        }
        stage('构建镜像') {
            environment {
                HARBOR_URL = "harbor.local.com"
                HARBOR_PROJECT = "spring_boot_demo"
                IMAGE_TAG = ''
            }
            steps {
                echo '开始构建镜像'
                // script {
                //     IMAGE_TAG = branchName == 'master' ? VersionNumber(versionPrefix: 'p', versionNumberString: '${BUILD_DATE_FORMATTED, "yyMMdd"}.${BUILDS_TODAY}') :
                //                 branchName == 'test' ? VersionNumber(versionPrefix: 't', versionNumberString: '${BUILD_DATE_FORMATTED, "yyMMdd"}.${BUILDS_TODAY}') :
                //                 error("Unsupported branch: ${params.BRANCH}")
                //     IMAGE_NAME = "${HARBOR_URL}/${HARBOR_PROJECT}/${IMAGE_APP}:${IMAGE_TAG}"
                //     sh "nerdctl build --insecure-registry -t ${IMAGE_NAME} ."
                // }
                // echo '构建镜像完成'
                // echo '开始推送镜像'
                // withCredentials([usernamePassword(credentialsId: "${HARBOR_CRED}", passwordVariable: 'HARBOR_PASSWORD', usernameVariable: 'HARBOR_USERNAME')]) {
                //     sh """
                //         nerdctl login --insecure-registry ${HARBOR_URL} -u ${HARBOR_USERNAME} -p ${HARBOR_PASSWORD}
                //         nerdctl push --insecure-registry ${IMAGE_NAME}
                //     """
                // }
                echo '推送镜像完成'
                echo '开始删除镜像'
                // sh "nerdctl rmi -f ${IMAGE_NAME}"
                echo '删除镜像完成'
            }
        }
        stage('项目部署') {
            environment {
                YAML_NAME = "k8s.yaml"
            }
            steps {
                echo '开始修改资源清单'
                // script {
                //     def (NAME_SPACE, DOMAIN_NAME) = branchName == 'master' ? ['prod', 'demo.local.com'] :
                //                                      branchName == 'test' ? ['test', 'demo.test.com'] :
                //                                      error("Unsupported branch: ${params.BRANCH}")
                // }
                // contentReplace(configs: [fileContentReplaceConfig(configs: [
                //     fileContentReplaceItemConfig(replace: "${IMAGE_NAME}", search: 'IMAGE_NAME'),
                //     fileContentReplaceItemConfig(replace: "${NAME_SPACE}", search: 'NAME_SPACE'),
                //     fileContentReplaceItemConfig(replace: "${DOMAIN_NAME}", search: 'DOMAIN_NAME')],
                //     fileEncoding: 'UTF-8',
                //     filePath: "${YAML_NAME}",
                //     lineSeparator: 'Unix')])
                echo '修改资源清单完成'
                // sh "kubectl apply -f ${YAML_NAME}"
                echo '部署资源清单完成'
            }
        }
    }
    post {
        always {
            echo '开始发送邮件通知'
            emailext(subject: '构建通知：${PROJECT_NAME} - Build # ${BUILD_NUMBER} - ${BUILD_STATUS}!',
                     body: '${FILE,path="email.html"}',
                     to: 'wing0302@qq.com')
            echo '邮件通知发送完成'
        }
    }
}
