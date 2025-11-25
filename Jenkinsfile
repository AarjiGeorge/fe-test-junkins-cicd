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

        // stage('Lint') {
        //     steps {
        //         sh 'npx ng lint'
        //     }
        // }

        // stage('Test') {
        //     steps {
        //         sh 'npx ng test --no-watch --no-progress --browsers=ChromeHeadless'
        //     }
        // }

        stage('Build') {
            steps {
                sh 'npx ng build --configuration=production --base-href=/'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sh '''
                    # Use /tmp (writable by Jenkins)
                    rm -rf /tmp/app-dist.new
                    mkdir -p /tmp/app-dist.new
                    cp -r dist/fe-angular-cicd/* /tmp/app-dist.new/

                    # Replace the shared volume contents
                    rm -rf /app-dist/*
                    cp -r /tmp/app-dist.new/* /app-dist/
                '''
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