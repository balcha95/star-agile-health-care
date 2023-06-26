pipeline {
    agent { label 'slave' }
	
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
	        maven "maven 3.8.8"
    }
    environment {	
		DOCKERHUB_CREDENTIALS=credentials('new_docker_id')
	}
    stages { 
        stage('SCM Checkout') {
            steps {
                // Get some code from a GitHub repository
                 git 'https://github.com/balcha95/star-agile-health-care.git'
            }
		}
        stage('Maven Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
		}
                stage("Docker build"){
            steps {
				sh 'docker version'
					sh "docker build -t balcha/healthcare_app:${BUILD_NUMBER} ."
					sh 'docker image list'
					sh "docker tag balcha/healthcare_app:${BUILD_NUMBER} balcha/healthcare_app:latest"
            }
        }
		stage('Login2DockerHub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
        stage('Approve - push Image to dockerhub'){
            steps{
                
                //----------------send an approval prompt-------------
                script {
                   env.APPROVED_DEPLOY = input message: 'User input required Choose "yes" | "Abort"'
                       }
                //-----------------end approval prompt------------
            }
        }
		stage('Push2DockerHub') {

			steps {
				sh "docker push balcha/healthcare_app:latest"
			}
		}
    }
}
