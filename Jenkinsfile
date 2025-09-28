pipeline {
    agent any
    
    environment {
        APP_DIR = '/home/ec2-user/apps/flask-backend'
        VENV_PATH = '/home/ec2-user/apps/flask-backend/venv'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out Flask backend code...'
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/flask-backend.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                sh '''
                    cd ${APP_DIR}
                    python3 -m venv ${VENV_PATH} || true
                    source ${VENV_PATH}/bin/activate
                    pip3 install -r requirements.txt
                '''
            }
        }
        
        stage('Deploy Application') {
            steps {
                echo 'Deploying Flask backend...'
                sh '''
                    cd ${APP_DIR}
                    pm2 stop flask-backend || true
                    pm2 start app.py --name flask-backend --interpreter python3
                    pm2 save
                '''
            }
        }
        
        stage('Health Check') {
            steps {
                echo 'Performing health check...'
                sh 'sleep 10'
                sh 'curl -f http://localhost:5000/api/health || exit 1'
            }
        }
    }
    
    post {
        success {
            echo 'Flask backend deployment completed successfully!'
        }
        failure {
            echo 'Flask backend deployment failed!'
        }
    }
}