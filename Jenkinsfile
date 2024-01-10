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
                script {
                    // Your build steps
                    sh 'mvn clean install'
                }
            }
        }
        stage('Copy Artifact') {
            steps {
                script {
                    def buildInfo = Artifactory.newBuildInfo()

                    def server = Artifactory.newServer url: ARTIFACTORY_URL, credentialsId: ARTIFACTORY_CREDENTIALS_ID

                    // Publish the build info to Artifactory
                    server.publishBuildInfo buildInfo

                    // Copy artifacts from the source project to the target repository
                    server.download spec: """{
                        "files": [
                            {
                                "pattern": "target/*.war",
                                "target": "${TARGET_REPO}/"
                            }
                        ]
                    }"""
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
