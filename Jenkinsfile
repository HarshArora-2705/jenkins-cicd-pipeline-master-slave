pipeline {
    
    agent {
        label "linuxbuildnode"
    }
    
    
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/HarshArora-2705/jenkins-cicd-pipeline-master-slave.git
                
            }
            
        }
        
        stage('Build by Maven Package') {
            steps {
                sh 'mvn clean package'
            }
            
        }
        
        
        stage('Build Docker OWN image') {
            steps {
                sh "sudo docker build -t  harshka2705/javaweb:${BUILD_TAG}  ."
                //sh 'whoami'
            }
            
        }
        
        
        stage('Push Image to Docker HUB') {
            steps {
                
                withCredentials([string(credentialsId: 'DOCKER_HUB_PWD', variable: 'DOCKER_HUB_PASS_CODE')]) {
    // some block
                 sh "sudo docker login -u harshka2705 -p $DOCKER_HUB_PASS_CODE"
}
               
               sh "sudo docker push harshka2705/javaweb:${BUILD_TAG}"
            }
            
        }
        
        
        stage('Deploy webAPP in DEV Env') {
            steps {
                sh 'sudo docker rm -f myjavaapp'
                sh "sudo docker run  -d  -p  8080:8080 --name myjavaapp   harshka2705/javaweb:${BUILD_TAG}"
                //sh 'whoami'
            }
            
        }
        
        
        stage('Deploy webAPP in QA/Test Env') {
            steps {
               
               sshagent(['QA_ENV_SSH_CRED']) {
    
                    sh "ssh  -o  StrictHostKeyChecking=no ec2-user@13.233.100.238 sudo docker rm -f myjavaapp"
                    sh "ssh ec2-user@13.233.100.238 sudo docker run  -d  -p  8080:8080 --name myjavaapp   harshka2705/javaweb:${BUILD_TAG}"
                }

            }
            
        }
        
        
         stage('QAT Test') {
            steps {
                
               // sh 'curl --silent http://13.233.100.238:8080/java-web-app/ |  grep India'
                
                retry(10) {
                    sh 'curl --silent http://13.233.100.238:8080/java-web-app/ |  grep India'
                }
            
               
            }
        }
          
        
         
         
        stage('approved') {
            steps {
                
            
            script {
                Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput

                if(userInput == true) {
                    // do action
                } else {
                    // not do action
                    echo "Action was aborted."
                }
            
                
            }
        }
        }
        
        
         
        
        stage('Deploy webAPP in Prod Env') {
            steps {
               
               sshagent(['QA_ENV_SSH_CRED']) {
    
                    sh "ssh  -o  StrictHostKeyChecking=no ec2-user@13.233.100.239 sudo docker rm -f myjavaapp"
                    sh "ssh ec2-user@13.233.100.239 sudo docker run  -d  -p  8080:8080 --name myjavaapp   harshka2705/javaweb:${BUILD_TAG}"

                }

            }
            
        } 
        
    }
    
}
