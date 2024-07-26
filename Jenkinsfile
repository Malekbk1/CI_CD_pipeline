def version = new Date().format("yyyyMMddHHmmss")
pipeline {
    agent any
	environment {
         SONARQUBE_CREDENTIALS = credentials('sonar-credentials')
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
	stage('SonarQube Analysis') {
           steps {
                 withSonarQubeEnv('sonarqube-10.6.0') {
                    sh "mvn sonar:sonar -Dsonar.login=${SONARQUBE_CREDENTIALS_USR} -Dsonar.password=${SONARQUBE_CREDENTIALS_PSW}"
                }
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
