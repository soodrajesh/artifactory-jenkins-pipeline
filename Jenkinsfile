pipeline {
    agent any

    environment {
        // Define your Artifactory credentials ID
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
        ARTIFACTORY_URL = "http://ec2-35-81-82-98.us-west-2.compute.amazonaws.com:8081/artifactory"
        ARTIFACTORY_REPO = "demo"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from your version control system
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build your Maven project
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'mvn test'
            }
        }

        stage('Deploy to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server ARTIFACTORY_URL, ARTIFACTORY_CREDENTIALS_ID
                    def rtMaven = Artifactory.newMavenBuild()
                    
                    rtMaven.tool = 'Maven'
                    rtMaven.deployer repo: ARTIFACTORY_REPO, server: server
                    rtMaven.deployer.deployArtifacts = false // Set to true if you want to deploy artifacts
                    
                    // Deploy the artifacts to Artifactory
                    rtMaven.deployer.deployArtifacts buildInfo
                }
            }
        }
    }
}
