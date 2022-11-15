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
                    url: 'https://github.com/Mark-1305/Complete-CI-CD-Kubernetes-ArgoCD.git'
                }
            }
        stage('Build Docker Image'){
                steps {
                    script{
                        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                            sh 'printenv'
                            sh 'sudo docker build -t "${IMAGE_NAME}" .'
                    }
                }
            }
        }    
        stage('Build Push Image') {
                steps {
                    script{
                        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                        sh    'sudo docker push "${IMAGE_NAME}": "$IMAGE_TAG"'
                        sh    'sudo docker push "${IMAGE_NAME}":latest'                            
                        }
                    }
                }
            }
        stage('Delete Docker Images'){
                steps {
                    sh "sudo docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "sudo docker rmi ${IMAGE_NAME}:latest"
                }
                          
            }
        }
    }
