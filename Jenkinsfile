pipeline{

	agent any

	stages {

		stage('Build') {

			steps {
				sh 'docker login ghcr.io --username vannh-glx --password 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'
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
