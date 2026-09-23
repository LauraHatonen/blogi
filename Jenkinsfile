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

}
stage('Nikto') {
            options {
                timeout(time: 15, unit: 'MINUTES')
            }
            steps {
                sh 'sleep 10'
                sh 'docker run --rm --link blog:blog hackllc/nikto:latest -h http://blog:3000/ || true'
            }
        }
    

