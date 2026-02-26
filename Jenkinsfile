pipeline {
    agent any

    stages {

        stage('Build Image') {
            steps {
                sh 'docker build -t minimal-frontend .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop minimal-app || true
                docker rm minimal-app || true
                docker run -d -p 3000:80 --name minimal-app minimal-frontend
                '''
            }
        }
    }
}
