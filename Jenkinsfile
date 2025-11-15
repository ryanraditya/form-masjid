pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                sh '''
                    rm -rf build
                    mkdir build
                    cp index.html build/
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing HTML...'
                sh 'grep -iq "<html" index.html'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Nginx...'
                sh '''
                    # Copy build result to deploy folder
                    rm -rf /tmp/deploy-form-masjid
                    mkdir -p /tmp/deploy-form-masjid
                    cp -r build/* /tmp/deploy-form-masjid/

                    # Restart container
                    docker stop form-masjid || true
                    docker rm form-masjid || true

                    docker run -d --name form-masjid -p 8080:80 \
                        -v /tmp/deploy-form-masjid:/usr/share/nginx/html \
                        nginx:alpine
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
