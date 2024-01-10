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
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: ARTIFACTORY_CREDENTIALS_ID, usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
                        def mavenHome = tool 'Maven3'
                        def mavenCMD = "${mavenHome}/bin/mvn"

                        sh "${mavenCMD} -f MyPhoenixApp/pom.xml clean install -e"
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
                        projectName: 'artifactory-jenkins-project2', // Replace with your source project name
                        selector: lastSuccessful()
                    )
                }
            }
        }
    }
}
