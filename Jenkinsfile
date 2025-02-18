pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = "DockerHub-access-token"
        REPO_NAME = "node-nginx-app"
    }

    stages { 

        stage('Clone Repository') {
            steps {
                withCredentials([string(credentialsId: 'GitHub-Token', variable: 'GITHUB_PAT')]) {
                    sh '''
                        echo "Cloning Repository..."
                        rm -rf $REPO_NAME  # Remove existing folder if present
                        git clone https://$GITHUB_PAT@github.com/TahirBaltee/$REPO_NAME.git
                    '''
                }
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                withCredentials([string(credentialsId: 'DockerHub-access-token', variable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "Logging into Docker Hub..."
                        echo "$DOCKER_PASS" | docker login -u tahirbaltee --password-stdin

                        echo "Building Docker Images..."
                        cd $REPO_NAME  # Ensure we are in the correct directory
                        docker-compose build

                        echo "Pushing Docker Images..."
                        docker-compose push
                    '''
                }
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    echo "Deploying Application..."
                    cd $REPO_NAME  # Ensure correct directory
                    docker-compose down || true  # Stop existing containers (ignore errors)
                    docker-compose up -d
                '''
            }
        }
    }  

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
