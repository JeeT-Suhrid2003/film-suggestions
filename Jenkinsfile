pipeline {
    agent any

    environment {
        APP_NAME = 'film-suggestions'
        DEPLOY_DIR = '/var/www/film-suggestions'
        EC2_USER = 'ubuntu'
        EC2_HOST = 'your-aws-ec2-public-ip-or-dns'
        NODE_ENV = 'production'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JeeT-Suhrid2003/film-suggestions.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test -- --watch=false'
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                sshagent(['aws-ec2-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} "mkdir -p ${DEPLOY_DIR}"
                        scp -o StrictHostKeyChecking=no -r . ${EC2_USER}@${EC2_HOST}:${DEPLOY_DIR}/release
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} "cd ${DEPLOY_DIR}/release && npm install --production && pm2 delete ${APP_NAME} || true && pm2 start npm --name ${APP_NAME} -- start"
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Application deployed successfully to AWS EC2.'
        }
        failure {
            echo 'Deployment failed. Please inspect logs.'
        }
    }
}
