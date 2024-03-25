pipeline {
  environment {
    registry = "pradeepnakalraju99/flask"
    registry_mysql = "pradeepnakalraju99/mysql"
    dockerImage = ""
  }

agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/pradeepnt/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Flask Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "dockerhub-id", url: "" ]) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }

  stage('Build mysql image') {
     steps{
        script { 
       withDockerRegistry([ credentialsId: "dockerhub2", url: "" ]) {
       sh 'docker build -t "pradeepnakalraju99/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
       
       sh 'docker push "pradeepnakalraju99/mysql:$BUILD_NUMBER"'
        }
      }}}
      
    stage('Push MySQL Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "dockerhub2", url: "" ]) {
            dockerImage.push("registry_mysql")
          }
        }
      }
    }
   // stage('Build mysql image') {
   //   steps{
   //     sh 'docker build -t "10.138.0.3:5001/mgsgoms/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
   //      sh 'docker push "10.138.0.3:5001/mgsgoms/mysql:$BUILD_NUMBER"'
   //      }
   //    }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
