pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/labiba-test/Rep-test.git'
        BUILD_DIR = 'dotnet_build'
        DEPLOY_DIR = 'C:/inetpub/wwwroot/Testgit' 
        SERVER_IP = '157.173.123.61' 
        CREDENTIALS_ID = 'server-credentials-id' 
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                script {
                    // Creating a build directory for .NET project
                    dir("${BUILD_DIR}") {
                        // Restore dependencies and build the project
                        bat 'dotnet restore'
                        bat 'dotnet build --configuration Release'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Publish the .NET app to prepare for deployment
                    dir("${BUILD_DIR}") {
                        bat 'dotnet publish -c Release -o publish_output'
                    }
                    
                    // Deploy to the server
                    bat """
                    powershell -Command "Copy-Item -Path .\\${BUILD_DIR}\\publish_output\\* -Destination \\\\${SERVER_IP}\\${DEPLOY_DIR} -Recurse -Force"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
