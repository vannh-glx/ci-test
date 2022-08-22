pipeline{

	agent any
	environment {
		GITHUB_TOKEN = credentials('github-token')
	}

	stages {
		stage('Prepare'){
			steps {
			     sh "git clone https://${env.GITHUB_TOKEN}@github.com/vannh-glx/ci-test.git"
			}
		}
		stage('Build') {
			steps {
				sh "docker login ghcr.io --username vannh-glx --password ${env.GITHUB_TOKEN}"
			    	sh 'docker build --tag ghcr.io/vannh-glx/flyte-workflow-image:v1 .'
			}
		}
	}

// 	post {
// 		always {
// 			sh 'docker logout'
// 		}
// 	}

}
