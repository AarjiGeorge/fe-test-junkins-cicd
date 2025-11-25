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
                    echo "→ Deploying to Nginx..."

                    # Copy new build directly into /app-dist (overwrite files)
                    cp -rf dist/fe-angular-cicd/* /app-dist/

                    # Optional: remove any leftover files from previous builds that are NOT in the new build
                    # (Only if you're concerned about stale files — often not needed for Angular apps)
                    echo "✅ Deployment completed."
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