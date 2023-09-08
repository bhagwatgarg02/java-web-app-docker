node{
    
   def buildNumber = BUILD_NUMBER
   
    stage("Git CheckOut"){
        git url: 'https://github.com/bhagwatgarg02/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
    } 
    
    stage("Build Dokcer Image") {
         sh "docker build -t bhagwatgarg/tomcat1:${buildNumber} ."
    }
    
    
    stage("Docker login and Push"){
        withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) 
       { 
        sh "docker login -u bhagwatgarg -p ${DockerHubPwd}"   
       }
        sh "docker push bhagwatgarg/tomcat1:${buildNumber}"
    }
        
        stage('Docker application as Docker container In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app dockerhandson/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
             sshagent(['webserver']) {
         sh "ssh -o StrictHostKeyChecking=no ubuntu@3.110.62.192 docker rm -f tomcatcontainer || true"
         sh "ssh -o StrictHostKeyChecking=no ubuntu@3.110.62.192 docker run -d -p 8080:8080 --name tomcatcontainer bhagwatgarg/tomcat1:${buildNumber}"
             }
       }
       
    }
}
