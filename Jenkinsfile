pipeline {
    agent any

    stages {
        stage('Git') {
            steps {
                git 'https://github.com/vaibhavkalel1/JobPortal-Automation.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t vaibhavkalel/jobportalautomation ."
                }
            }
        }
        stage('Cretae Docker Container') {
            steps {
                script {
                    bat "docker run -d --name jobportalautomationcontainer -p 8000:8000  vaibhavkalel/jobportalautomation"
                }
            }
        }
        stage('Push Docker Images to Docker Hub') {

            steps {

                script {

                    // Log in to Docker Hub using Jenkins credentials

                    withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {

                        bat "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"

 

                        // Push the Docker images to your Docker Hub repository
                     
                        bat 'docker push vaibhavkalel/jobportalautomation'

                    }

                }

            }
        }
        stage('Download Minikube for Windows') {
            steps {
                bat 'curl -Lo minikube.exe https://storage.googleapis.com/minikube/releases/latest/minikube-windows-amd64.exe'
            }
        }
        stage('Install Minikube') {
            steps {
                bat 'move minikube.exe C:\\Users\\12826\\.jenkins\\workspace\\JobPortal-Automation'
                bat 'setx PATH "%PATH%;C:\\minikube"'
            }
        }
        stage('Start Minikube') {
            steps {
                script {
                    // Define Minikube installation path (update as needed)
                    def minikubePath = 'C:\\Users\\12826\\minikube.exe'

 

                    // Start Minikube
                    bat "cd C:\\Users\\12826\\.jenkins\\workspace\\JobPortal-Automation && ${minikubePath} start --driver=docker"
                }
            }
        }
        /*stage('Minikube status') {
            steps {
                script {
                    bat "minikube status"
                }
            }
        }*/
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    bat "kubectl apply -f deployment.yml"
                    bat "kubectl get deployment"
                }
            }
        }
        stage('Create NodePort Service') {
            steps {
                script {
                    bat "kubectl apply -f service.yml"
                    bat "kubectl get svc"
                }
            }
        }
        stage('Expose NodePort 8000') {
            steps {
                script {
                    bat "kubectl expose deployment jobportal-app-deployment1 --type=NodePort --port=8000"
                }
            }
        }
        stage('Get URL') {
            steps {
                script {
                    bat "minikube service jobportal-app-service1 --url"
                }
            }
        }
        /*stage('Get URL and play with Application') {
            steps {
                script {
                    bat "minikube service social-app-service1"
                }
            }
        }*/
    }
}
