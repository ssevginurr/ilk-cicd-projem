pipeline {
    agent any

    environment {
        DOCKER_HOST = 'unix:///var/run/docker.sock' // Host Docker daemon ile iletişim
    }

    stages {
        stage('Kodu GitHub\'dan Çek') {
            steps {
                // SSH ile bağlandığımız GitHub repo
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          userRemoteConfigs: [[
                              url: 'git@github.com:ssevginurr/ilk-cicd-projem.git',
                              credentialsId: 'github-ssh'
                          ]]
                ])
            }
        }

        stage('Docker İmajını Oluştur (Build)') {
            steps {
                // Host Docker daemon kullanılıyor
                sh 'docker build -t benim-sitem .'
            }
        }

        stage('Projeyi Sunucuda Çalıştır (Deploy)') {
            steps {
                // Eski container varsa sil, sonra yeni çalıştır
                sh 'docker rm -f benim-sitem-container || true'
                sh 'docker run -d -p 8085:80 --name benim-sitem-container benim-sitem'
            }
        }
    }
}
