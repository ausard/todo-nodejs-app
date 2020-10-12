pipeline{
    agent any
    options{
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
        timestamps()
    }
    environment {
        registryBackend = "ausard/todo-backend"
        registryFrontend = "ausard/todo-frontend"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    stages {
        stage('Preparation'){
           steps{
                cleanWs()
                //Installing kubectl in Jenkins agent
                // sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
                // sh 'chmod +x ./kubectl'
                // && sudo mv kubectl /usr/local/sbin'

                //Clone git repository
                git branch: 'main', 
                    url: 'https://github.com/ausard/todo-nodejs-app.git'
           }   
        }
        stage('Building images') {
            steps{
                dir('backend') {
                    script {
                        dockerImageBackend = docker.build registryBackend + ":latest"
                    }
                }
                dir('frontend') {
                    script {
                        dockerImageFrontend = docker.build registryFrontend + ":latest"
                    }
                }
             }
          }
          stage('Push Images') {
              steps{
                  script {
                       docker.withRegistry( '', registryCredential){                            
                       dockerImageBackend.push()
                       dockerImageFrontend.push()
                      }
                   }
                } 
           }
           stage('Deploying into k8s'){
            steps{
                sh 'kubectl apply -f depoy/k8s'
                // withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'jenkins-deployer-credentials', namespace: '', serverUrl: 'https://172.17.0.2') {
                    
                // }                
            }
        }
    }
}