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
                // Your repository checkout steps here
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/soodrajesh/artifactory-jenkins-pipeline.git']]])
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    // Build your project (replace this with your build steps)
                    def mavenHome = tool 'Maven3'
                    sh "${mavenHome}/bin/mvn -f MyPhoenixApp/pom.xml clean install -e"

                    // Copy the latest .war file from the source repository to demo2
                    copyArtifacts(
                        projectName: 'artifactory-jenkins-project',
                        filter: '*.war',
                        target: '/var/lib/jenkins/.m2/repository/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/'
                    )

                    // Rename the copied file to match the target repository path
                    sh "mv /var/lib/jenkins/.m2/repository/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/*.war /var/lib/jenkins/.m2/repository/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/MyPhoenixApp-1.0-SNAPSHOT.war"

                    // Upload the .war file to the target repository
                    sh "curl -u ${ARTIFACTORY_CREDENTIALS_ID} -XPUT \"${ARTIFACTORY_URL}/${TARGET_REPO}/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/MyPhoenixApp-1.0-SNAPSHOT.war\" --data-binary @/var/lib/jenkins/.m2/repository/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/MyPhoenixApp-1.0-SNAPSHOT.war"
                }
            }
        }
    }

    post {
        success {
            // This block will be executed only if the build is successful
            echo 'Build successful!'
        }
    }
}
