pipeline {
    agent any

    environment {
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
        ARTIFACTORY_URL = 'http://ec2-35-81-82-98.us-west-2.compute.amazonaws.com:8081/artifactory'
        ARTIFACTORY_REPO = 'demo'
        TARGET_REPO = 'demo2'
    }

    stages {
        // Previous stages as per your pipeline

        stage('Deploy to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server ARTIFACTORY_URL, ARTIFACTORY_CREDENTIALS_ID
                    def rtMaven = Artifactory.newMavenBuild()

                    // Maven build steps as per your pipeline

                    // Deploy the artifacts to Artifactory
                    def buildInfo = rtMaven.deployer.deployArtifacts()

                    // Add a post-build step to copy the .war file to another repository if the build is successful
                    if (currentBuild.resultIsBetterOrEqualTo('SUCCESS')) {
                        step([$class: 'CopyArtifact', projectName: 'YourOtherJobName', filter: 'MyPhoenixApp/target/*.war', target: 'demo2/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/'])
                    }
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
