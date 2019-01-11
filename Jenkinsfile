node{
      
      stage('SCM Checkout'){
         git 'https://github.com/LovesCloud/javatomcatappwithdocker'
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

         
   stage('Ansible'){      
         stage('ansible'){
        ansiblePlaybook credentialsId: 'tomcatdeploymentserver', disableHostKeyChecking: true, installation: 'ansible 2.7.5', playbook: '${WORKSPACE}/deploy.yml'
      }      
         
   }
 }

