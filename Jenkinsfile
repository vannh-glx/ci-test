pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS_PSW=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
		DOCKERHUB_CREDENTIALS_USR=vannh-glx
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build --tag ghcr.io/vannh-glx/flyte-workflow-image:v1 .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push ghcr.io/vannh-glx/flyte-workflow-image:v1'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}