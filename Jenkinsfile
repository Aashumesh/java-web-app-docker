node{
    
   def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
    
       git url: 'https://github.com/Aashumesh/java-web-app-docker.git', branch: 'master'
        }
        
    stage("Maven Clean Package"){
        
        def mavenHome= tool name: "Maven",type: "maven"
        sh "${mavenHome}/bin/mvn clean package"
    }
       
    stage("Build Docker Image"){
    
        sh "docker build -t ashwinimesh/jave-web-app:${buildNumber} ."
    }
    stage("Docker Login And Push"){
        withCredentials([string(credentialsId: 'docker_Hub_Pwd', variable: 'docker_Hub_Pwd')]) {
            sh "docker login -u ashwinimesh -p ${docker_Hub_Pwd}"
    // some block
    }
        sh "docker push ashwinimesh/jave-web-app:${buildNumber}"
    }
    
    stage("Deploy Application As Docker Container In Docker Deployment Server"){
       sshagent(['Docker-Dev-Server-SSH']) {
           
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.0.13 docker rm -f javawebappcontainer || true" 
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.0.13 docker run -d -p 80:80 ashwinimesh/jave-web-app:${buildNumber}"
                    
       }    
    }
}
