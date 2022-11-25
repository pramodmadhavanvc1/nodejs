node{
   stage('SCM Checkout'){
       git credentialsId: 'git', url: 'https://github.com/pramodmadhavanvc1/nodejs.git'
   }
   
   stage('Docker build app image'){
     sh 'ssh 10.0.2.8 "docker build -t pramodmadhavanvc1/nodejs:1.5 /git/docker/"'
   }
   
   stage('Docker login and push to HUB'){
     withCredentials([string(credentialsId: 'docker-hub-pramodmadhavan', variable: 'docker-hub-pramodmadhavan')]) {
       sh "ssh 10.0.2.8 'docker login -u pramodmadhavan -p ${docker-hub-pramodmadhavan}'"
   }
     sh 'ssh 10.0.2.8 "docker push pramodmadhavanvc1/nodejs:1.5"'
   }
   
   stage('creating POD on kube cluster'){
     sh 'ssh root@10.0.2.4 "kubectl create -f /kubernetes/nodeapp-deployment.yaml"'
     sh 'ssh root@10.0.2.4 "kubectl create -f /kubernetes/nodeapp-service.yaml"'
   }
}
