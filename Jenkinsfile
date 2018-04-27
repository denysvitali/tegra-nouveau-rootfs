node {
  docker.image("dvitali/build-container:latest").inside() {
    try {
      stage('Pull'){
        checkout scm
      }
      stage('Pull Submodules'){
        sh 'git submodule update --init --recursive'
      }
	  	stage('Prepare'){
        sh 'export TOP=$PWD'
        sh 'export ARCH=aarch64'
        sh './scripts/download-gcc'
        sh './scripts/download-rootfs'
	  	}

      stage('Generate RootFS'){
        sh 'ls -la $HOME'
        sh 'echo PWD: $PWD'
        sh 'ls -la /app'
        sh './scripts/build-linux'
        sh './scripts/prepare-rootfs'
        sh './scripts/build-drm'
        sh './scripts/build-mesa'
        sh './scripts/generate-rootfs'
      }
      stage('Archive Artifacts'){
        archiveArtifacts 'out/rootfs.tar.gz'
      }
	  } finally {
      cleanWS()
    }
  }
}
