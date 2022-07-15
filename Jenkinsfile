pipeline {
   stage('SCM Checkout'){
       git 'https://github.com/jahirshawon/pipeline'
   }
   stage('Mvn Package'){
     def mvnHome = tool name: 'Maven', type: 'maven'
     def mvnCMD = "${mvnHome}/bin/mvn"
     sh "${mvnCMD} clean package"
   }
   stage('Build Docker Image'){
     sh 'docker build -t jahirshawon/my-app:2.0.1 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword')]) {
        sh "docker login -u jahirshawon -p ${dockerhubpassword}"
     }
     sh 'docker push jahirshawon/my-app:2.0.1'
   }
   stage('Run Container on Dev Server'){
     def dockerRun = 'docker run -p 8080:8080 -d --name my-app jahirshawon/my-app:2.0.1'
     sshagent(['dockerserver4']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun}"
     }
   }
}
