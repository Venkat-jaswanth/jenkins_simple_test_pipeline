pipeline {
    agent any

    stages {

        stage('Build Image') {
            steps {
                sh 'docker build -t 2023BCD0021-jenkinassignment .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop 2023bcd0021-jenkinassignment || true
                docker rm 2023bcd0021-jenkinassignment || true
                docker run -d -p 3000:80 --name 2023bcd0021-jenkinassignment 2023bcd0021-jenkinassignment
                '''
            }
        }

        // stage('Push to Docker Hub - using pass username') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
        //             sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
        //             sh 'docker tag minimal-frontend $DOCKER_USER/minimal-frontend:latest'
        //             sh 'docker push $DOCKER_USER/minimal-frontend:latest'
        //         }
        //     }
        // }
        stage('Push to Docker Hub - using token') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_TOKEN')]) {
                    sh 'echo "$DOCKER_TOKEN" | docker login -u venkatjaswanth --password-stdin'
                    sh 'docker tag 2023bcd0021-jenkinassignment venkatjaswanth/2023bcd0021-jenkinassignment:latest'
                    sh 'docker push venkatjaswanth/2023bcd0021-jenkinassignment:latest'
                }
            }
        }
    }
}
