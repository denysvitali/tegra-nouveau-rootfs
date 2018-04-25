pipeline {
	agent {
		docker {
			image 'ubuntu:latest'
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
        sh 'apt-get update'
        sh 'apt-get install -y sudo wget curl git python'
				sh 'sh -c "curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo"'
        sh 'chmod a+x /usr/bin/repo'
        sh 'git config --global color.ui true'
        sh 'repo init -u https://github.com/pixelc-linux/rootfs-generator -b tegra-mesa -m tegra-nouveau.xml > /dev/null'
        sh 'repo sync -j4 -c > /dev/null'
        sh 'export TOP=$PWD'
        sh 'export ARCH=aarch64'
        sh 'apt-get install -y proot git build-essential wget phablet-tools autoconf automake libtool libc6-i386 lib32stdc++6 lib32z1 pkg-config libwayland-dev bison flex bc u-boot-tools glib-2.0 libffi-dev xutils-dev python-mako intltool libxml2-dev > /dev/null'
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
