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
                }
            }
        }
        stage('Copy Artifact') {
            steps {
                script {
                    // Copy artifacts from the source project to the target project
                    copyArtifacts(
                        filter: '*.war',
                        projectName: 'correct_source_project_name', // Replace with the correct source project name
                        selector: lastSuccessful()
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
