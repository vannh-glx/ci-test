pipeline{
	agent any
	environment {
		PATH="/var/lib/jenkins/.local/bin:/var/lib/jenkins/bin:$PATH"
		GITHUB_CREDENTIALS=credentials('github-credential')
		REGISTRY="ghcr.io"
		USERNAME="vannh-glx"
		IMAGE_NAME="test-workflow"
		PROJECT_NAME="flytetester"
		DOMAIN_NAME="development"
		REPO="ci-test"
		FULL_IMAGE_NAME="${REGISTRY}/${USERNAME}/${IMAGE_NAME}"
		CONFIG_PATH="/var/lib/jenkins/.flyte/config.yaml"
	}
	stages {
        stage('Prepare'){
	        steps {
	            sh "git clone https://${env.GITHUB_TOKEN}@github.com/vannh-glx/ci-test.git"
	            dir(${REPO}){
	                script {
	                    env.TAG = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
	                }
	            }
	        }
		}

		stage('Build image') {
			steps {
			    dir(${REPO}){
			        sh 'docker build --tag ${FULL_IMAGE_NAME}:${TAG} .'
			    }
			}
		}
		
		stage('Package workflow') {
			steps {
			    dir(${REPO}){
			        sh 'pyflyte --pkgs flyte.workflows package --image ${FULL_IMAGE_NAME}:${TAG} -f'
			    }
			}
		}
		
		stage('Login docker registry'){
			steps {
				sh 'echo ${GITHUB_CREDENTIALS_PSW} | docker login ghcr.io -u ${GITHUB_CREDENTIALS_USR} --password-stdin'
			}
		}
		
		stage('Push image') {
			steps {
				sh 'docker push ${FULL_IMAGE_NAME}:${TAG}';
			}
		}
		
		stage('Register workflow') {
			steps {
			    dir(${REPO}){
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
