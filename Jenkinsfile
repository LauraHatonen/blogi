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
    steps {
        sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image --timeout 15m --severity HIGH,CRITICAL blog:latest'
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
stage('Nikto') {
    steps {
        // Annetaan sovellukselle 10 sekuntia aikaa käynnistyä
        sh 'sleep 10'
        // Ajetaan Nikto nopeammalla tuning-asetuksella (-Tuning 1x eli vain mielenkiintoiset tiedostot/aukot)
        sh 'docker run --rm --link blog:blog hackllc/nikto:sha-e108110 -h http://blog:3000/ -Tuning 1x || true'
    }
}