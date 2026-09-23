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
stage('Nikto Scan') {
            steps {
                // Odotetaan hetki, että blog-sovellus ehtii käynnistyä
                sh 'sleep 5'
                // Suoritetaan Nikto-skannaus käyttäen host-verkkoasetusta
                sh 'docker run --rm --network host sullo/nikto -h http://localhost:3000/'
            }
        }
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