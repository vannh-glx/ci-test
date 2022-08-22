pipeline{

	agent any
	environment {
		GITHUB_CREDENTIALS=credentials('github-credential')
		REPO="ghcr.io/vannh-glx"
		IMAGE="test-workflow"
		VERSION="v2"
		TAG="${REPO}/${IMAGE}:${VERSION}"
	}

	stages {
		stage('Build') {
			steps {
			    	sh 'docker build --tag $TAG .'
			}
		}
		stage('Login'){
			steps {
				sh 'echo $GITHUB_CREDENTIALS_PSW | docker login ghcr.io -u $GITHUB_CREDENTIALS_USR --password-stdin'
			}
		}
		stage('Push') {
			steps {
				sh 'docker push $TAG';
			}
		}
	}

	  post {
	    always {
	      echo 'Done pipeline'
	      cleanWs deleteDirs: true
	    }
	  }

}
