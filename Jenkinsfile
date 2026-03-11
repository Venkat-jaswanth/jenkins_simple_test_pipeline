pipeline {
    agent any

    stages {

        stage('Build Image') {
            steps {
                sh 'docker build -t 2023bcd0021-jenkinassignment .'
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
        stage('Deploy to Cloud (EC2)') {
            steps {
                // Use the SSH key stored in Jenkins credentials
                sshagent(['cloud-ssh-key']) {
                    // Replace 'ubuntu@<YOUR_CLOUD_IP_ADDRESS>' with your actual server IP and username
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@13.60.163.82 "
                        # 1. Pull the latest image from Docker Hub
                        docker pull venkatjaswanth/2023bcd0021-jenkinassignment:latest
                        
                        # 2. Stop and remove the old running container
                        docker stop my-cloud-app || true
                        docker rm my-cloud-app || true
                        
                        # 3. Run the new container on the cloud server
                        docker run -d -p 80:80 --name my-cloud-app venkatjaswanth/2023bcd0021-jenkinassignment:latest
                    "
                    '''
                }
            }
        }
    }
}
