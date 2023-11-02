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
           sh 'gradle init'
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
        stage("SSH Into k8s Server") {
         steps {
                def remote = [:]
                remote.name = 'master'
                remote.host = '16.16.182.102'
                remote.user = 'ubuntu'
                remote.allowAnyHosts = true
         }
         }
        stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
         steps {
                    sshPut remote: remote, from: 'deployment.yml', into: '.'
                }
        }
        stage('Deploy spring boot') {
         steps {
                  sshCommand remote: remote, command: "kubectl apply -f deploym>
        }
        }
    }
    }
