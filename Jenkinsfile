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
                        // Your build steps
                        sh 'mvn clean install' // Replace with your actual build command
                    }
                }
            }
        }

        stage('Copy Artifact') {
            steps {
                script {
                    def server = Artifactory.server ARTIFACTORY_CREDENTIALS_ID
                    def buildInfo = Artifactory.newBuildInfo()

                    server.download(
                        "${env.JOB_NAME}/${BUILD_NUMBER}/archive/*.war",
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
            // Add post-success actions if needed
        }
        failure {
            echo 'Pipeline failed!'
            // Add post-failure actions if needed
        }
    }
}
