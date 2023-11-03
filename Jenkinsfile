node {
 
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

    stage("login") {
        sh 'docker login -u rahmafeidi -p 11937703rahma'
    }

    stage("Push Image to Docker Hub"){
		sh 'docker push  rahmafeidi/testspring:latest'
    }

    stage("SSH Into k8s Server") {
                def remote = [:]
                remote.name = 'master'
                remote.host = '16.16.182.102'
                remote.user = 'ubuntu'
                def sshCredentialsId = 'ssh_key'
                remote.allowAnyHosts = true

        stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
                    sshPut remote: remote, from: 'deployment.yml', into: '.'
        }

        stage('Deploy spring boot') {
                  sshCommand remote: remote, command: " kubectl apply -f deployment.yml"
        }
    }

}
    
