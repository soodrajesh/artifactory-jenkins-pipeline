pipeline {
    agent any

    environment {
        ARTIFACTORY_REPO = 'demo'
        TARGET_REPO = 'demo2'
        ARTIFACTORY_URL = 'http://ec2-35-94-82-123.us-west-2.compute.amazonaws.com:8081/artifactory'
        ARTIFACTORY_CREDENTIALS_ID = 'artifactory-cred'
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
                    // Artifactory credentials
                    def credentials = credentials('artifactory-cred')
                    def username = credentials.username
                    def password = credentials.password

                    // Source and target URLs
                    def sourceUrl = "${ARTIFACTORY_URL}/${ARTIFACTORY_REPO}/${env.JOB_NAME}/${BUILD_NUMBER}/archive/*.war"
                    def targetUrl = "${ARTIFACTORY_URL}/${TARGET_REPO}/${env.JOB_NAME}/"

                    // cURL command to copy artifacts
                    def curlCommand = """
                        curl -vvv -u ${username}:${password} \
                        -X COPY "${sourceUrl}" \
                        "${targetUrl}" \
                        --header "Destination: ${targetUrl}"
                    """

                    // Execute the cURL command
                    sh curlCommand.trim()
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
