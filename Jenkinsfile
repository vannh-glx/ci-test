pipeline{

	agent any
	environment {
	    PATH="/var/lib/jenkins/.local/bin:/var/lib/jenkins/bin:$PATH"
		GITHUB_CREDENTIALS=credentials('github-credential')
		REGISTRY="ghcr.io"
		USERNAME="vannh-glx"
		IMAGE_NAME="test-workflow"
		IMAGE_VERSION="v2"
		PROJECT_NAME="flytetester"
		DOMAIN_NAME="development"
		WORKFLOW_VERSION="v1"
		FULL_IMAGE_NAME="${REGISTRY}/${USERNAME}/${IMAGE_NAME}"
		CONFIG_PATH="/var/lib/jenkins/.flyte/config.yaml"
		
		
	}
	stages {
	    stage('Prepare'){
			steps {
		        sh "git clone https://env.GITHUB_CREDENTIALS_PSW@github.com/vannh-glx/ci-test.git"
		        dir('ci-test'){
		            script {
		                env.TAG = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
		            }
 			    }
			}
		}
		stage('Build and package') {
			steps {
			    dir('ci-test'){
			        sh 'docker build --tag ${FULL_IMAGE_NAME}:${TAG} .'
			        sh 'pyflyte --pkgs flyte.workflows package --image ${FULL_IMAGE_NAME}:${TAG} -f'
			    }
			}
		}
		stage('Login'){
			steps {
				sh 'echo ${GITHUB_CREDENTIALS_PSW} | docker login ghcr.io -u ${GITHUB_CREDENTIALS_USR} --password-stdin'
			}
		}
		stage('Push') {
			steps {
				sh 'docker push ${FULL_IMAGE_NAME}:${TAG}';
			}
		}
		stage('Register') {
			steps {
			    dir('ci-test'){
			        sh 'flytectl register files --config ${CONFIG_PATH} --project ${PROJECT_NAME} \
			        --domain ${DOMAIN_NAME} --archive flyte-package.tgz --version ${TAG}'; 
			    }
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
