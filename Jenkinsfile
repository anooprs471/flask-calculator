pipeline {
  agent {
    kubernetes {
      label 'kubeagent'
    }

  }
  environment{
    DOCKER_USERNAME = credentials('DOCKER_USERNAME')
    DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
  }
  stages {
    stage('Fetch') {
      steps {
        git(branch: 'main', url: 'https://github.com/anooprs471/flask-calculator.git')
      }
    }

    stage('docker login') {
		steps{
			sh(script: """
            docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
			""", returnStdout: true) 
		}
    }

    stage('Build-Image') {
      steps {
        sh '''
        pack build anooprs471/test-flask-calculator:${BUILD_NUMBER} --path . --builder paketobuildpacks/builder:base
        '''
      }
    }
	
    stage('docker push') {
		steps{
			sh(script: """
            docker push anooprs471/test-flask-calculator:${BUILD_NUMBER}
			""")
		}
    }

    stage('deploy') {
      steps {
        sh 'echo "Place Holder For Deployment"'
      }
    }

  }
}