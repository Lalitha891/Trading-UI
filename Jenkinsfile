pipeline {
    agent any

    environment {
        NODE_HOME = '/usr/bin' // Adjust path if different
        PATH = "${NODE_HOME}:${PATH}"
    }

    stages {
        stage('Install Node.js and PM2 (if needed)') {
            steps {
                sh '''
                    which node || curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
                    which node || sudo apt-get install -y nodejs
                    which pm2 || sudo npm install -g pm2
                '''
            }
        }

        stage('Git checkout') {
            steps {
                git 'https://github.com/betawins/Trading-UI.git'
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
                    cd build
                    pm2 delete Trading-UI || true
                    pm2 --name Trading-UI start npm -- start
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
