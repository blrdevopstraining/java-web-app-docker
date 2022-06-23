node{
     
    stage('SCM Checkout'){
        git credentialsId: 'githubpassword', url: 'https://github.com/blrdevopstraining/java-web-app-docker.git'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "maven-3.8.6", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t blrdevopstraining/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u blrdevopstraining -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push blrdevopstraining/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app blrdevopstraining/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ec2-user@13.233.60.209 docker stop java-web-app || true'
          sh 'ssh  ubuntu@13.233.60.209 docker rm java-web-app || true'
          sh 'ssh  ubuntu@13.233.60.209 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@13.233.60.209 ${dockerRun}"
       }
       
    }
     
     
}
