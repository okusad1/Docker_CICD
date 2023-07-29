pipeline {
	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('josiokoko')
	}

	stages {

		stage("Git Checkout") {
			steps {
				git branch: 'main', url: 'https://github.com/josiokoko/cicd.git'
			}
		}

		stage("Docker Build") {
			steps{
				sh "docker build -t josiokoko/timeserver ."
			}
		}

		stage("Authenticate") {
			steps{
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				echo "Login complete..."
			}
		}


		stage("Push to Registry") {
			steps{
				script {
					sh 'docker tag josiokoko/timeserver docker.io/josiokoko/efosaserver:$BUILD_ID'
					sh 'docker push docker.io/josiokoko/efosaserver:$BUILD_ID'
				}
			}
		}

		stage("Cleanup") {
			steps{
				sh 'docker rmi -f $(docker image ls -aq)'
				sh 'docker logout'
			}
		}


	}
}