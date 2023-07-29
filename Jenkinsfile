pipeline {
	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('okusad1')
	}

	stages {

		stage("Git Checkout") {
			steps {
				git branch: 'master', url: 'https://github.com/okusad1/Docker_CICD.git'
			}
		}

		stage("Docker Build") {
			steps{
				sh "docker build -t okusad1/timeserver ."
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
					sh 'docker tag okusad1/timeserver docker.io/okusad1/okusad1server:$BUILD_ID'
					sh 'docker push docker.io/okusad1/okusad1server:$BUILD_ID'
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