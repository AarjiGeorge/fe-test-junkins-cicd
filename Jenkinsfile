pipeline {
    agent any

    tools {
        nodejs "NodeJS-18"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                sh 'ng lint'
            }
        }

        stage('Test') {
            steps {
                sh 'ng test --no-watch --no-progress --browsers=ChromeHeadless'
            }
        }

        stage('Build') {
            steps {
                sh 'ng build --configuration=production --base-href=/'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                // Clear previous build
                sh 'rm -rf /app-dist/*'
                // Copy new build (replace "my-angular-app" with your actual project name)
                sh 'cp -r dist/my-angular-app/* /app-dist/'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed! App is live at http://<your-server-IP>'
        }
        failure {
            echo '❌ Build or deployment failed.'
        }
    }
}