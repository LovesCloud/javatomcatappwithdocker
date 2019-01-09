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
         sh 'docker build -t rajnikhattarrsinha/javadockerdemo:2.0.0 .'
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
              sh "docker login -u rajnikhattarrsinha -p ${dockerPWD}"
         }
        sh 'docker push rajnikhattarrsinha/javadockerdemo:2.0.0'
      }

   //stage('Stop running containers'){        
      //   def listContainer='sudo docker ps'
        // def scriptRunner='sudo ./stopscript.sh'
         //def stopContainer='sudo docker stop $(docker ps -a -q)'
         //sshagent(['tomcatdeploymentserver']) {
              //sh "ssh -o StrictHostKeyChecking=no rajni@35.237.148.63 ${scriptRunner}"   
             // sh 'sudo su'
           //    sh "${scriptRunner}"
               //
      //   }
    //} 
   stage('Pull Docker Image and Deploy'){        
         
            def dockerContainerName = 'javadockerdemo1-$JOB_NAME-$BUILD_NUMBER'
            def dockerRun= "sudo docker run -p 8080:8080 -d --name ${dockerContainerName} rajnikhattarrsinha/javadockerdemo:2.0.0"         
            sshagent(['tomcatdeploymentserver']) {
              sh "ssh -o StrictHostKeyChecking=no rajni@35.237.148.63 ${dockerRun}"   
                 // sh "${dockerRun}" 
         }
   }
 }

