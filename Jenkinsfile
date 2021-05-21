pipeline { 
    environment { 
        registry = "veekrum/nodeproject" 
        registryCredential = "veekrum" 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Cloning our Git') { 
            steps { 
                git 'https://github.com/veekrum/node-todo-frontend.git' 
            }
        } 
        stage('Build') {
                sh 'npm install'
        }
        stage('Test') {
                sh 'npm test'
        }
        stage('Building our image') { 

            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 

            }
        } 
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    }
}


