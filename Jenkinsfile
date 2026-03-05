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

        // stage('Push to Docker Hub') {
        //     steps {
        //         // IMPORTANT: You need to create a credential in Jenkins named 'dockerhub-credentials' (Type: Username with password)
        //         // containing your Docker Hub username and password/access token.
        //         withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
        //             sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
        //             sh 'docker tag minimal-frontend $DOCKER_USER/minimal-frontend:latest'
        //             sh 'docker push $DOCKER_USER/minimal-frontend:latest'
        //         }
        //     }
        // }
        stage('Push to Docker Hub - directly') {
            steps {
                    sh 'docker tag minimal-frontend venkatjaswanth/minimal-frontend:latest'
                    sh 'docker push venkatjaswanth/minimal-frontend:latest'
                
            }
        }
    }
}
