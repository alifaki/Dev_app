pipeline {
    agent any
    stages {
        stage('Inject Secrets') {
            steps {
                sh '''
                    cp /home/kist/docker/Dev_app/.env .
                    mkdir -p backend
                    cp /home/kist/docker/Dev_app/backend/.env backend/
                    cp /home/kist/docker/Dev_app/backend/config.json backend/
                '''
            }
        }
        stage('Rebuild & Deploy') {
            steps { 
                sh 'docker-compose down -v || true' 
                sh 'docker-compose up -d --build' 
            }
        }
    }
}