peline { 
    environment { 
        registry = "mrseyh/python-flask-exp" 
        registryCredential = 'dockerhub_id' 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Test') {
            steps {
                sh 'echo Testing your application'
            }
        }
        stage('Push the image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
	        } 
                } 
            }
         stage ('Deploy the image'){
             steps {
                  sshagent(credentials: ['deploy-dev']) {
                    sh '''
                        [ -d ~/.ssh ] || mkdir ~/.ssh && chmod 0700 ~/.ssh
                        ssh-keyscan -t rsa,dsa 192.168.0.230 >> ~/.ssh/known_hosts
                        ssh remote-host@192.168.0.230

                        docker image pull mrseyh/python-flask-exp:26
                        docker run -d mrseyh/python-flask-exp:26
                       '''
                } 
             }
         } 
        } 
}

