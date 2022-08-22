pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS_PSW="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"
		DOCKERHUB_CREDENTIALS_USR="vannh-glx"
	}

	stages {

		stage('Build') {

			steps {
				sh 'echo "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" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" ghcr.io --password-stdin'
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
