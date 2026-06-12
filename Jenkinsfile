pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'cat index.html'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to app-server...'
                sshagent(['app-server-ssh']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no devops@192.168.56.20 "sudo mkdir -p /var/www/html"
                        scp index.html devops@192.168.56.20:/var/www/html/index.html
                    '''
                }
            }
        }
        
        stage('Health Check') {
            steps {
                echo 'Running health check...'
                sh 'curl -s http://192.168.56.20 | grep DevOps'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
