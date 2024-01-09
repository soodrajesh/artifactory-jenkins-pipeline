pipeline {
    agent any

    environment {
        ARTIFACTORY_REPO = 'demo'
        TARGET_REPO = 'demo2'
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
        ARTIFACTORY_URL = 'http://ec2-35-81-82-98.us-west-2.compute.amazonaws.com:8081/artifactory'
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
                    // Your build steps go here
                    // For example:
                    sh 'mvn clean install'

                    // Stash the artifacts
                    stash includes: 'MyPhoenixApp/target/*.war', name: 'myArtifacts'
                }
            }
        }

        stage('Copy Artifact') {
            steps {
                script {
                    // Unstash and copy the artifacts to the target repository
                    unstash 'myArtifacts'
                    sh "cp MyPhoenixApp/target/*.war /var/lib/jenkins/jobs/${TARGET_REPO}/workspace/"
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
