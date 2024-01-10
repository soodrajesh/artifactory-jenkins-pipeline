pipeline {
    agent any

    environment {
        ARTIFACTORY_REPO = 'demo'
        TARGET_REPO = 'demo2'
        ARTIFACTORY_URL = 'http://ec2-35-94-82-123.us-west-2.compute.amazonaws.com:8081/artifactory'
        
        // Derive credentials ID based on Artifactory URL
        ARTIFACTORY_CREDENTIALS_ID = "${ARTIFACTORY_URL.hashCode()}"
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
                    // Copy artifacts from the source project to the target project
                    copyArtifacts(
                        filter: '*.war',
                        projectName: 'MyPhoenixApp', // Replace with the correct source project name
                        selector: lastSuccessful()
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
