def version = new Date().format("yyyyMMddHHmmss")
pipeline {
    agent any
	environment {
        SONAR_TOKEN = '9bfe06984c607e7fe7ea7d6e0b62efda'  
    }
	tools { maven "Maven3" }   
      stages {
        stage('checkout github repositoy') {
            steps {
                echo 'pulling';
                git branch:'main',url : 'https://github.com/Malekbk1/CI_CD_pipeline.git';
            }
        }
	
       stage('build Maven ') { 
	     steps { 
		sh 'mvn clean install' 
             }
        }
      
        stage('RUN tests') { 
		parallel { 
	      stage('Unit test ') 
		{ steps { sh 'mvn test' } } 
            }
	}
		 stage('Docker image '){
            steps {
                 sh 'docker build -t malek132/malek .'
            }
        }
        stage('push to DockerHub'){
            steps { 
		        withCredentials([usernamePassword(credentialsId: 'dockerhubdev', passwordVariable: 'PASSWORD', usernameVariable: 'USER')])
		        {
                    sh 'docker login -u ${USER} -p ${PASSWORD}'
                    sh 'docker push malek132/malek'
                    
                }
       }
       }
        stage('DockerCompose') {
        
            steps {
		    withEnv(["PATH=$PATH:~/.local/bin"]){
				    sh 'docker-compose up -d'
                    }
                          
        }
    
      }
}
}
