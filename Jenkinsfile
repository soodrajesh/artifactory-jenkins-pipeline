pipeline {
    agent any

    environment {
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
        ARTIFACTORY_URL = "http://ec2-35-81-82-98.us-west-2.compute.amazonaws.com:8081/artifactory"
        ARTIFACTORY_REPO = "demo"
        TARGET_REPO = "demo2"
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
                    rtMaven.deployer.deployArtifacts buildInfo

                    // Add a post-build step to copy artifacts to another repository if the build is successful
                    if (currentBuild.resultIsBetterOrEqualTo('SUCCESS')) {
                        copyArtifactsToTargetRepo(server)
                    }
                }
            }
        }
    }

    post {
        success {
            // This block will be executed only if the build is successful
            echo 'Build successful!'
            copyArtifactsToTargetRepo(Artifactory.server ARTIFACTORY_URL, ARTIFACTORY_CREDENTIALS_ID)
        }
    }
}

def copyArtifactsToTargetRepo(server) {
    echo 'Copying artifacts to target repository...'
    def sourceRepoPath = "your-artifactory-repo/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/"
    def targetRepoPath = "${TARGET_REPO}/com/dept/app/MyPhoenixApp/1.0-SNAPSHOT/"

    // Artifactory REST API to copy artifacts
    def copyRequest = """
        {
            "dry": false,
            "repositories": [
                {
                    "srcRepoPath": "${sourceRepoPath}",
                    "dstRepoPath": "${targetRepoPath}"
                }
            ]
        }
    """

    def copyResponse = server.artifactoryService.artifactoryManager
        .post("/api/copy/${ARTIFACTORY_REPO}", copyRequest)

    echo "Copy response: ${copyResponse}"
}
