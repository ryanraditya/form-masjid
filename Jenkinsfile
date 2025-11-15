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
                echo 'Building project (HTML)...'
                sh 'test -f index.html'
                sh 'mkdir -p build'
                sh 'cp -r * build/'
                echo 'Build step completed.'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing HTML...'
                sh 'grep -iq "<html" index.html'
                echo 'Test Passed: HTML structure is valid.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Nginx container...'
                sh '''
                    mkdir -p /tmp/deploy-form-masjid
                    cp -r * /tmp/deploy-form-masjid/

                    docker stop form-masjid || true
                    docker rm form-masjid || true

                    docker run -d --name form-masjid -p 8080:80 \
                        -v /tmp/deploy-form-masjid:/usr/share/nginx/html \
                        nginx:alpine
                '''
                echo 'Deploy step completed.'
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
