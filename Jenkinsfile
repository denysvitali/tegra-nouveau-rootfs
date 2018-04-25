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
				sh 'sh -c "curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo"'
        sh 'chmod a+x /usr/bin/repo'
        sh 'git config --global color.ui true'
        sh 'repo init -u https://github.com/pixelc-linux/rootfs-generator -b tegra-mesa -m tegra-nouveau.xml '
        sh 'repo sync -j4 -c'
        sh 'export TOP=$PWD'
        sh 'export ARCH=aarch64'
			}
		}

    stage('Generate RootFS'){
      steps {
        sh './scripts/download-gcc'
        sh './scripts/download-rootfs'
        sh './scripts/build-linux'
        sh './scripts/prepare-rootfs'
        sh './scripts/build-drm'
        sh './scripts/build-mesa'
        sh './scripts/generate-rootfs'
      }
    }
	}
	post {
		always {
			cleanWs()
		}
	}
}
