pipeline {
    agent any
  
     environment {
       DOCKERHUB_CREDENTIALS = credentials('dockerhub')
     }
     stages { 
        stage("Git Clone"){
 
            git credentialsId: 'github', url: 'https://github.com/rahmafeidi/spring.git'
        }

        stage('Gradle Build') {

          sh './gradlew build'

        }

	stage("Docker build"){
		sh 'docker version'
		sh 'docker build -t testspring .'
		sh 'docker image list'
		sh 'docker tag testspring rahmafeidi/testspring:latest'
	    }

        stage('Login') {
	      steps {
		sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
	      }
	    }
        stage("Push Image to Docker Hub"){
		sh 'docker push  rahmafeidi/testspring:latest'
	    }

	stage("SSH Into k8s Server") {
		def remote = [:]
		remote.name = 'master'
		remote.host = '16.16.182.102'
		remote.user = 'ubuntu'
		remote.allowAnyHosts = true

	stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
		    sshPut remote: remote, from: 'deployment.yml', into: '.'
		}

	stage('Deploy spring boot') {
		  sshCommand remote: remote, command: "kubectl apply -f deployment.yml"
        }
    }
  }
}

