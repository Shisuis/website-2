pipeline {
    agent none
    environment{
        DOCKERHUB_CREDENTIALS = credentials("54411234-0f26-4780-9560-56caf1cedd29")
    }
    
    stages{
        stage('git'){
            agent {
                label "kube"
            }
            steps{
                script{
                     git 'https://github.com/Shisuis/website-2.git'
                }
            }
        }
        stage('docker'){
            agent{
                label "kube"
            }
            steps{
                script{
                    sh 'sudo docker build -t shisuis/project2 .'
                    sh 'sudo docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}'
                    sh 'sudo docker push shisuis/project2'
                }
            }
        }
        stage('kubernetes'){
            agent{
                label "kube"
            }
            steps{
                script{
                    sh 'kubectl delete deploy my-app'
                    sh 'kubectl apply -f deploy.yaml'
                    sh 'kubectl delete service my-service'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}
