node{
      
      stage('SCM Checkout'){
         git 'https://github.com/rajnikhattarrsinha/java-tomcat-maven-example'
      }
  
      stage('Mvn Build'){
         // Get maven home path and build
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
         sh "${mvnHome}/bin/mvn package"
      }
      
     stage ('Test'){
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
         sh "${mvnHome}/bin/mvn verify; sleep 3"
      }  
      
    stage('Build Docker Image'){
         sh 'docker build -t rajnikhattarrsinha/tomcatdocker09:2.0.0 .'
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
              sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
        sh 'docker push rajnikhattarrsinha/tomcatdocker09:2.0.0'
      }

       stage('Stop running containers'){        
       //def listContainer='sudo docker ps'
       def scriptRunner='sudo ./stopscript.sh'
       // def stopContainer='sudo docker stop $(docker ps -a -q)'
      // sshagent(['dockerdeployserver2']) {
       sshagent(['tomcatdeploymentserver']) {             
       // sshagent(['dockergcpserver']) {
              sh "ssh -o StrictHostKeyChecking=no rajni@35.227.106.13 ${scriptRunner}"            
         }
    } 
  
   stage('Pull Docker Image and Deploy'){        
         
            def dockerContainerName = 'docker-pipeline-$BUILD_NUMBER'
            def dockerRun= "sudo docker run -p 8080:8080 -d --name ${dockerContainerName} rajnikhattarrsinha/tomcatdocker09:2.0.0"         
           //sshagent(['dockerdeployserver2']) {
        // sshagent(['dockergcpserver']) {
              sshagent(['tomcatdeploymentserver']) {   
                 // // sh "ssh -o StrictHostKeyChecking=no ubuntu@54.144.118.163 ${dockerRun}" 
               sh "ssh -o StrictHostKeyChecking=no rajni@35.227.106.13 ${dockerRun}"
              
         }
   }
 }


