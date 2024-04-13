pipeline {
  environment {
    KUBE_TOKEN = 'eyJhbGciOiJSUzI1NiIsImtpZCI6IjM5V01xbGdSTGFfMnd3SmxhOEcxMHBvWHl4NlpoU3pnSTBEUXZWZElxSE0ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImplbmtpbnMtYWRtaW4tdG9rZW4iLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiamVua2lucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImU4N2M4MDc0LWU3YTctNGY1Zi04Nzg2LWEwNWI0MmYwNDMxZiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmplbmtpbnMifQ.FncGgS0_kabx1cGmiyM9a1e7CiZmahtgE6veYqdtQAi-ijF5yEtP8sXWth3AfGF8sbjS4Y9I5qsTtQ5raJoPVBeYX5qFzBj-8Ti9oxmabEr2aojJ7Wkv4GCXFoMDyE13pPpHcZGs-Pf2xC7NaBbUEym1f1hOI_5mUgq8KjX6ygEMxFxe5x1rhVD3AmE2qEozubW061KbmWe2pRjm8bFCu2VrtTE1Ciy5w5bKeh7bSVDj_KWdT9t6X8PPv12l5zbl8wu9atdRbz_cD2RyutOhutN_ZzN3O3ZiSoT5vU8Fix8LftdmdYmkRzI-dbiFQsl2NAJ3H3VZMVvs35beud2N8A'
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
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "", kubeconfig: [token: "${KUBE_TOKEN}"])
        }
      }
    }
  //   stage('Deploy App') {
  //     steps {
  //       script {
  //         sh """
  //           kubectl config set-credentials jenkins --token=\${KUBE_TOKEN}
  //           kubectl apply -f frontend.yaml
  //           """
  //       }
  //     }
  // }
}
}
