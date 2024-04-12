pipeline {
  environment {
    KUBE_TOKEN = credentials('kubernetes-token')
    registry = "docker.io/pradeepnakalraju99/flask"
    registry_mysql = "docker.io/pradeepnakalraju99/mysql"
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
       withDockerRegistry([ credentialsId: "dockerhub-id", url: "" ]) {
       sh 'docker build -t "docker.io/pradeepnakalraju99/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
       
       sh 'docker push "docker.io/pradeepnakalraju99/mysql:$BUILD_NUMBER"'
        }
      }}}
      
    stage('Push MySQL Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "dockerhub-id", url: "" ]) {
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
    // stage('Deploy App') {
    //   steps {
    //     script {
    //       kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kubernetes-token")
    //     }
    //   }
    // }
    stage('Deploy App') {
      steps {
        script {
          sh """
            kubectl config set-credentials jenkins --token=\${KUBE_TOKEN}
            kubectl apply -f frontend.yaml --context pradeep
            """
        }
      }
  }
}
}
