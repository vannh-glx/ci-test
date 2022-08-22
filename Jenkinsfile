pipeline{

	agent any

	stages {

		stage('Build') {

			steps {
				scripts{
				    sh 'docker login ghcr.io --username vannh-glx --password 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';
				    sh 'docker build --tag ghcr.io/vannh-glx/flyte-workflow-image:v1 .'
				}
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
