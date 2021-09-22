pipeline { 
    environment { 
        registry = "asamaniya/project-hangon-javamavenapp" 
        registryCredential = 'docker' 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Cloning our Git') { 
            steps { 
                git 'https://github.com/amitkumar20201/project-hang-out.git' 
            }
        } 
        stage('Java Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
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
        stage('Deploy') { 
            steps { 
                sh "docker pull asamaniya/project-hangon-javamavenapp"
                sh "docker rm demo-default" 
            }
        }
        stage('RUN') { 
            steps { 
                sh "docker run -d --name demo-default -p 8090:8090 -p 8091:8091 asamaniya/project-hangon-javamavenapp" 
            }
        }
    }
}

