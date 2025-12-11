pipeline {
    agent any

    tools {
        nodejs "node20"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sshagent (credentials: ['deploy-ec2']) {
                    sh '''
                        # Copy build files to server
                        scp -o StrictHostKeyChecking=no -r build/* ubuntu@65.2.184.34:/var/www/html/

                        # Restart service on server
                        ssh -o StrictHostKeyChecking=no ubuntu@65.2.184.34 "sudo systemctl restart myapp"
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
