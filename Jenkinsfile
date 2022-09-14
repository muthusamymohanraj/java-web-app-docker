node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/muthusamymohanraj/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    }
    
    stage('Build Docker Image'){
        sh 'docker build -t 9884453217/myproject .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker_hub_password', variable: 'Dockerpassword')]) {
          sh "docker login -u 9884453217 -p ${Dockerpassword}"
        }
        sh 'docker push 9884453217/myproject'
     }
     
     stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8181:8080 --name myproject 9884453217/myproject'
         
         sshagent(['Docker_ssh_pwd']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@13.233.89.19 docker stop myproject || true'
          sh 'ssh  ubuntu@13.233.89.19 docker rm myproject || true'
          sh 'ssh  ubuntu@13.233.89.19 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@13.233.89.19 ${dockerRun}"
       }



}

}
