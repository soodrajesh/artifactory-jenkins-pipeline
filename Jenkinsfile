pipeline {
    agent any

    environment {
        ARTIFACTORY_REPO = 'demo'
        TARGET_REPO = 'demo2'
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
        ARTIFACTORY_URL = 'http://ec2-35-94-82-123.us-west-2.compute.amazonaws.com:8081/artifactory'
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

        stage('Copy to Target Repo') {
            steps {
                script {
                    // Copy artifact to the target repository
                    rtUpload (
                        serverId: "${ARTIFACTORY_CREDENTIALS_ID}",
                        spec: """{
                            "files": [
                                {
                                    "pattern": "MyPhoenixApp/target/*.war",
                                    "target": "${ARTIFACTORY_REPO}/${TARGET_REPO}/"
                                }
                            ]
                        }"""
                    )
                }
            }
        }

        // Add more stages as needed

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
