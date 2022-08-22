pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS_PSW="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"
		DOCKERHUB_CREDENTIALS_USR="vannh-glx"
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build --tag ghcr.io/vannh-glx/flyte-workflow-image:v1 .'
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
