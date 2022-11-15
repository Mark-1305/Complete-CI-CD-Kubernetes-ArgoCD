pipeline {
    agent any
    environment  {
        DOCKERHUB_USERNAME = "manoharshetty507"
        APP_NAME = "jenkins-kube-argocd"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    stages{
        stage('Clean Workspace'){
                steps {
                    script{
                        cleanWs()
                    }
                }
            }
        stage('Print Build Number'){
                steps {
                     sh "echo ${BUILD_NUMBER}"
            }
        }
        stage(' Checkout SCM'){
                steps {
                    git branch: 'main', 
                    credentialsId: 'github', 
                    url: 'https://github.com/ManoharShetty507/Sample-Flask-App-Jenkins-Kubernetes-Argo-CD.git'
                }
            }
        stage('Build Docker Image'){
                steps{
                    script{
                        docker.withRegistry('',)
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                }
            }
        stage('Build Push Image'){
                steps{
                    script{
                        docker.withRegistry('', REGISTRY_CREDS){
                            docker_image.push("${BUILD_NUMBER}")
                            docker_image.push('latest')                            
                        }
                    }
                }
            }
        stage('Delete Docker Images'){
                steps{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_NAME}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                          
            }
        }
    }
}
