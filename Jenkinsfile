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
    stage('Nikto Security Scan') {
            steps {
                script {
                    // Run Nikto and save the output as an XML report
                    sh 'nikto -h http://your-target-url.local -o nikto-report.xml -Format xml || true'
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