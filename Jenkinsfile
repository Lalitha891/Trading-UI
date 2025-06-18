pipeline {
    agent any

    environment {
        NODE_HOME = '/usr/bin' // Adjust if Node is in a different location
        PATH = "${NODE_HOME}:${env.PATH}"
    }

    stages {
        stage('Install Node.js and PM2 (if needed)') {
            steps {
                sh '''
                    which node || curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
                    which node || apt-get install -y nodejs
                    which pm2 || npm install -g pm2
                '''
            }
        }

        stage('Git checkout') {
            steps {
                git 'https://github.com/Lalitha891/Trading-UI.git'
            }
        }

        stage('Install and Build') {
            steps {
                sh '''
                    npm install
                    npm audit fix || true
                    npm run build
                '''
            }
        }

        stage('Deploy with PM2') {
            steps {
                sh '''
                    pm2 delete Trading-UI || true
                    pm2 --name Trading-UI start npm -- start
                    pm2 save
                '''
            }
        }
    }

    post {
        failure {
            echo '🚫 Build or deployment failed.'
        }
        success {
            echo '✅ Application deployed successfully!'
        }
    }
}
