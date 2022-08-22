pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS_PSW="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"
		DOCKERHUB_CREDENTIALS_USR="vannh-glx"
	}

	stages {

		stage('Build') {

			steps {
				sh 'echo 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 | docker login ghcr.io --username vannh-glx --password-stdin'
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
