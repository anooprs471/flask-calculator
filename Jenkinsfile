pipeline {
  agent { label 'ec2-ubuntu-node' }
  
  stages {
    stage('Fetch') {
      steps {
        git(branch: 'main', url: 'https://github.com/anooprs471/flask-calculator.git')
      }
    }

    stage('Build-Image') {
      steps {
        sh "pack build anooprs471/test-flask-calculator:${BUILD_NUMBER} --path . --builder paketobuildpacks/builder:base"}
    }
	
    stage('docker push') {
		steps{
			withCredentials([usernamePassword(credentialsId: 'Dockerhub_creds_username_password', passwordVariable: 'DOCKER_REGISTRY_PWD', usernameVariable: 'DOCKER_REGISTRY_USER')]){
			sh "docker login -u $DOCKER_REGISTRY_USER -p $DOCKER_REGISTRY_PWD"
			sh "docker push anooprs471/test-flask-calculator:${BUILD_NUMBER}"
			}
			}
    }

    stage('Deploy App to Kubernetes') {     
      steps {
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" flask-calc.yaml'
            sh 'kubectl apply -f flask-calc.yaml'
	    sh 'kubectl apply -f flask-calc-ingress.yaml'
      }
    }
  }
}
