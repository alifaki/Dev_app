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
                sh 'docker-compose down -v || true' # remove volumes
                sh 'docker-compose up -d --build' # rebuild images and start
            }
        }
        stage('health check') {
            steps {
                sh 'sleep 5; curl -f http://localhost:3000/health || exit 1'
            }
        }
        stage('integration test') {
            steps {
                sh 'docker exec app-backend npm run test:integration'
            }
        }
        post {
            failure {
                slackSend(color: 'danger', message: "Dev_app deploy failed")
            }
        }
    }
}