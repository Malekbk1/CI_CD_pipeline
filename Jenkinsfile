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
                    docker.image('maven:3.8.6-openjdk-11').inside('-v /var/run/docker.sock:/var/run/docker.sock') {
                        sh 'mvn clean install'
		    }
            }
        }
        stage('RUN tests') {
            parallel {
                  stage('Unit test ') {
            steps {
               
                sh 'mvn test'
            }
        }
          stage('Sonarqube Analysis ') {
            steps {
                withSonarQubeEnv('sonarqube-8.9.7'){
                sh 'mvn sonar:sonar'
                }
                }
            }
            
        }
               
                
            }
        
        stage('Docker image '){
            steps {
                 sh 'docker build -t rihabhn/backendappimage .'
            }
        }
        stage('push to DockerHub'){
            steps { 
		        withCredentials([usernamePassword(credentialsId: 'dockerhubdev', passwordVariable: 'PASSWORD', usernameVariable: 'USER')])
		        {
                    sh 'docker login -u ${USER} -p ${PASSWORD}'
                    sh 'docker push rihabhn/backendappimage'
                    
                }
       }
       }
        stage('DockerCompose') {
        
            steps {
				    sh 'docker-compose up -d'
                    }
                          
        }
        stage('Notify') {
            steps {
                script {
                    slackSend (color: '#36a64f', message: 'The build has completed successfully', tokenCredentialId: 'slack', channel: 'pfa')
                        }
                    }
        }
        
        
    }
}
