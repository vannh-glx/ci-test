pipeline{

	agent any
	environment {
		GITHUB_CREDENTIALS=credentials('github-credential')
		REGISTRY="ghcr.io"
		USERNAME="vannh-glx"
		IMAGE="test-workflow"
		IMAGE_VERSION="v2"
		PROJECT="flytetester"
		DOMAIN="development"
		WORKFLOW_VERSION="v1"
		TAG="${REGISTRY}/${USERNAME}/${IMAGE}:${IMAGE_VERSION}"
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
		stage('Package') {
			steps {
				sh 'pyflyte --pkgs flyte.workflows package --image $TAG -f';
			}
		}
		stage('Push') {
			steps {
				sh 'docker push $TAG';
			}
		}
		stage('Register') {
			steps {
				sh 'flytectl register files --config ~/.flyte/config.yaml --project $PROJECT --domain $DOMAIN --archive flyte-package.tgz --version $WORKFLOW_VERSION';
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
