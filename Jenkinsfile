pipeline {
    agent any

    environment {
        NODE_VERSION = node    // Match the tool name in Jenkins
    }

    tools {
        nodejs NODE_VERSION
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
                sh 'npm test || true'     // remove "|| true" if test failures should stop the pipeline
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying application..."
                // Example deploy command:
                // sh 'scp -r dist/* ubuntu@your-server:/var/www/myapp'
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
