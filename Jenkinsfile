pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'cekout kode'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'build kode tapi ini html'
            }
        }

        stage('Test') {
            steps {
                echo 'cek html'
                
                sh '''
                    if [ ! -f index.html ]; then
                        echo "❌ File index.html tidak ditemukan!"
                        exit 1
                    fi

                    echo "✔ File index.html ditemukan."

                    # Cek tag HTML dasar (optional)
                    if grep -qi "<html>" index.html; then
                        echo "✔ Struktur HTML valid."
                    else
                        echo "⚠ Tidak ditemukan tag <html>. Lanjut tapi mohon dicek."
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'deploy ke nginx'

                sh '''
                    echo "stop container lama jika ada"
                    docker stop form-masjid || true
                    docker rm form-masjid || true

                    echo "jalankan container baru..."
                    docker run -d --name form-masjid \
                        -p 8085:80 \
                        -v ${WORKSPACE}:/usr/share/nginx/html \
                        nginx

                    echo "berhasil deploy"
                '''
            }
        }
    }

    post {
        success {
            echo 'pipeline berhasil'
        }
        failure {
            echo 'Pipeline gagal'
        }
    }
}
