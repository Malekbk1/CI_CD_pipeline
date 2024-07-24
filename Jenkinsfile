def version = new Date().format("yyyyMMddHHmmss")
pipeline {
    agent any
      stages {
        stage('checkout github repositoy') {
            steps {
                echo 'pulling';
                git branch:'main',url : 'https://github.com/Malekbk1/CI_CD_pipeline.git';
            }
        }
	
        stage('build Maven ') {
            steps {
                script {
                    docker.image('maven:3.8.6-openjdk-11').inside {
                        sh 'mvn clean install'
		    }
		}
	    }
        }
      }
}
      
