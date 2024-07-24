def version = new Date().format("yyyyMMddHHmmss")
pipeline {
    agent any
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
		
    }
}
