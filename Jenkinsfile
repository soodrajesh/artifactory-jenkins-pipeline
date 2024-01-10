pipeline {
    agent any
    environment {
        ARTIFACTORY_REPO = 'demo'
        TARGET_REPO = 'demo2'
        ARTIFACTORY_URL = 'http://ec2-35-94-82-123.us-west-2.compute.amazonaws.com:8081/artifactory'
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                dir('MyPhoenixApp') {
                    script {
                        sh 'mvn clean install'
                    }
                }
            }
        }
        stage('Copy Artifact') {
            steps {
                script {
                    def server = Artifactory.server ARTIFACTORY_URL
                    def buildInfo = Artifactory.newBuildInfo()
                    server.download(
                        "${env.JOB_NAME}/${env.BUILD_NUMBER}/archive/*.war",
                        buildInfo
                    )
                    server.upload(
                        buildInfo,
                        "${TARGET_REPO}/${env.JOB_NAME}/",
                        "*.war"
                    )
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
