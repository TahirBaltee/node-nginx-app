pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = "DockerHub-access-token"
    }

    stages { 

        stage('Clone Repository') {
    steps {
        withCredentials([string(credentialsId: 'GitHub-Token', variable: 'GITHUB_PAT')]) {
            sh 'git clone https://$GITHUB_PAT@github.com/TahirBaltee/node-nginx-app.git'
        }
    }
}

        stage('Build and Push Docker Images') {
            steps {
                sh '''
                docker-compose build
                docker-compose push
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                docker-compose down
                docker-compose up -d
                '''
            }
        }
    }  
}
