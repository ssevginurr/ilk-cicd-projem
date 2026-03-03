pipeline {
    agent any
    
    stages {
        stage('Kodu GitHub\'dan Çek') {
            steps {
                checkout scm
            }
        }
        
        stage('Docker İmajını Oluştur (Build)') {
            steps {
                sh 'docker build -t benim-sitem .'
            }
        }
        
        stage('Projeyi Sunucuda Çalıştır (Deploy)') {
            steps {
                sh 'docker rm -f benim-sitem-container || true'
                sh 'docker run -d -p 8085:80 --name benim-sitem-container benim-sitem'
            }
        }
    }
}
