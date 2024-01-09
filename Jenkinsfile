pipeline {
    agent any

    environment {
        ARTIFACTORY_REPO = 'demo'
        TARGET_REPO = 'demo2'
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
        ARTIFACTORY_URL = 'http://ec2-35-81-82-98.us-west-2.compute.amazonaws.com:8081/artifactory'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Your build steps go here
                    // For example:
                    sh 'mvn clean install'
                }
            }
        }

        stage('Copy Artifact') {
            steps {
                script {
                    def sourcePath = "${ARTIFACTORY_REPO}/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/MyPhoenixApp-1.0-SNAPSHOT.war"
                    def targetPath = "${TARGET_REPO}/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/MyPhoenixApp-1.0-SNAPSHOT.war"
                    copyArtifacts filter: '*.war', fingerprintArtifacts: true, projectName: sourcePath, target: targetPath
                }
            }
        }
    }
}
