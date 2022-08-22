pipeline{

	agent any
	environment {
		GITHUB_CREDENTIALS=credentials('github-credential')
	}

	stages {
		stage('Build') {
			steps {
				sh 'echo $GITHUB_CREDENTIALS_PSW | docker login ghcr.io -u $GITHUB_CREDENTIALS_USR --password-stdin'
			    	sh 'docker build --tag ghcr.io/vannh-glx/flyte-workflow-image:v2 .'
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
