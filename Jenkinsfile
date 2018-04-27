pipeline {
	agent {
		docker {
			image 'dvitali/build-container:latest'
		}
	}
  options {
    timeout(time: 1, unit: 'HOURS')
  }
	stages {
    stage('Pull Submodules'){
      steps {
        sh 'git submodule update --init --recursive'
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
        sh 'ls -la $HOME'
        sh 'echo PWD: $PWD'
        sh 'ls -la /app'
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
