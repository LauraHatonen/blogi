pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                sh 'git pull origin main'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build --pull --rm -f "Dockerfile" -t blog:latest "."'
            }
        }
       stage('Trivy') {
}
        stage('Nikto') {
            steps {
                // Odotetaan 5 sekuntia, että blog-sovellus ehtii käynnistyä
                sh 'sleep 5'
                // Suoritetaan Nikto-skannaus haluamallasi Docker-kuvalla
                sh 'docker run --rm --link blog:blog hackllc/nikto:sha-e108110 -h http://blog:3000/ || true'
            }
        }
    }
}
        stage('Run') {
            steps {
                sh 'docker stop blog || true'
                sh 'docker rm blog || true'
                sh 'docker run -d -p 3000:3000 --name blog blog'
            }
        }
    }
}