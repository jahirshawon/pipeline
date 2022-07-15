node{
   stage('SCM Checkout'){
       git 'https://github.com/jahirshawon/pipeline'
   }
   stage('mvn Package'){
     def mvnHome = tool name: 'Maven', type: 'maven'
     def mvnCMD = "${mvnHome}/bin/mvn"
     sh "${mvnCMD} clean package"
   }
   stage('build Docker Image'){
     sh 'docker build -t jahirshawon/my-app:2.0.2 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword')]) {
        sh "docker login -u jahirshawon -p ${dockerhubpassword}"
     }
     sh 'docker push jahirshawon/my-app:2.0.2'
   }
   stage('Run Container on Dev Server'){ 
     def dockerRun = 'docker run -p 8080:8080 -d --name my-app jahirshawon/my-app:2.0.2'
     def dockerRun1 = 'docker rm -f my-app'
     sshagent(['dockerserver4']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun1}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun}"
     }
   }
}
