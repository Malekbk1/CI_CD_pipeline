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
		
    
	stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-10.6.0') {
                    // Run SonarQube scanner with the token
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }
}
