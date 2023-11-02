pipeline {
    agent any
    
    tools {
        // Specify the name of the Gradle installation defined in Jenkins
        gradle 'gradle'
    }

  
     environment {
       DOCKERHUB_CREDENTIALS = credentials('dockerhub')
     }
     stages { 
        stage("Git Clone"){
         steps {
 
            git credentialsId: 'github', url: 'https://github.com/rahmafeidi/spring.git'
        }
        }

        stage('Gradle Build') {
         steps {

          sh 'gradle build'

        }
        }

	stage("Docker build"){
	 steps {
		sh 'docker version'
		sh 'docker build -t testspring .'
		sh 'docker image list'
		sh 'docker tag testspring rahmafeidi/testspring:latest'
	    }
	    }

        stage('Login') {
	      steps {
		sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
	      }
	    }
        stage("Push Image to Docker Hub"){
            steps {
		sh 'docker push  rahmafeidi/testspring:latest'
	    }
           }
	
  }
}

