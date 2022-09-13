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
    
        sh "docker build -t ashwinimesh/java-web-app:${buildNumber} ."
    }
    
    stage("Docker Login And Push"){
       withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Passwd')]) {
            sh "docker login -u ashwinimesh -p ${Docker_Hub_Passwd}"
    }
            sh "docker push ashwinimesh/java-web-app:${buildNumber}"
    }
    
    stage("Deploy Application As Docker Container In Docker Deployment Server"){
       sshagent(['Docker-Dev-Server-SSH']) {
           
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.34.2 docker rm -f javawebappcontainer || true" 
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.34.2 docker run -d -p 80:80 ashwinimesh/java-web-app:${buildNumber}"
                    
       }    
    }
}

