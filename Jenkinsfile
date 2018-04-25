pipeline {
	agent {
		docker {
			image 'dvitali/build-container:latest'
			args '-v $HOME/build:/kernel --privileged'
		}
	}
	options {
    		skipDefaultCheckout(true)
	}
	stages {
		stage('Pull') {
			steps {
				checkout scm
			}
		}
		stage('Prepare'){
			steps {
        sh 'export TOP=$PWD'
        sh 'export ARCH=aarch64'
        sh './scripts/download-gcc'
        sh './scripts/download-rootfs'
			}
		}

    stage('Generate RootFS'){
      steps {
        sh './scripts/build-linux'
        sh './scripts/prepare-rootfs'
        sh './scripts/build-drm'
        sh './scripts/build-mesa'
        sh './scripts/generate-rootfs'
      }
    }
    stage('Archive Artifacts'){
      steps {
        archiveArtifacts 'out/rootfs.tar.gz'
      }
    }
	}
	post {
		always {
			cleanWs()
		}
	}
}
