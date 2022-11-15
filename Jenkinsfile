pipeline {
    agent any
    parameters{
    choice(name: 'action', choices: 'create\ndelete', description: 'Create and Delete the pods')
    }
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
                            sh 'sudo docker build -t "${DOCKERHUB_USERNAME}/${APP_NAME}" .'
                    }
                }
            }
        }    
        stage('Build Push Image') {
                steps {
                    script{
                        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                        /*sh    "sudo docker push '${DOCKERHUB_USERNAME}/${APP_NAME}':'${IMAGE_TAG}'"/* */
                        sh "sudo docker push '${DOCKERHUB_USERNAME}/${APP_NAME}':latest"                            
                        }
                    }
                }
            }
        stage('Delete Docker Images'){
                steps {
                    /*sh "sudo docker rmi '${DOCKERHUB_USERNAME}/${APP_NAME}':'${IMAGE_TAG}'"*/
                    sh "sudo docker rmi '${DOCKERHUB_USERNAME}/${APP_NAME}':latest"
                }
                          
            }

/*  Kubernetes Deployment without AUthentication             
        stage('Kubernetes Deployment - Dev'){
                steps{
                withKubeConfig([credentialsId: 'kubeconfig']) {
                sh "sed -i 's#replace#${DOCKERHUB_USERNAME}/${APP_NAME}':latest#g' deployment.yml"
                sh "kubectl apply -f deployment.yml "
                } 
            }   
        }    
    }
}
*/
        stage('Kubernetes Deployments_In Local_Cluster'){
            when { expression {params.action == 'create'}}
            steps{
                withKubeConfig([credentialsId: 'kubeconfig']){
                    script{
                    def apply = false
                    try{
                        input message: 'please confirm the apply to iniate the deployment', ok: 'Ready to apply the config'
                        apply = true
                    }
                    catch (err){
                        apply = false
                        CurrentBuild.result = 'UNSTABLE'
                    }
                    if (apply){
                        sh """
                        kubectl apply -f deployment.yml
                        """;
                        }  
                    } 
                }               
            }
        }   
    }
}
