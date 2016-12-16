#!groovy

node {


   // stage 'build' {
      git 'https://github.com/Yanivro/rapid-app.git'

    //Authenticate with docker hub in order to push artifact into it

        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '459bf397-3910-4c22-8d0b-55107eadcbb5',
        usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD']]) {

          sh 'docker login --username $DOCKER_USER --password $DOCKER_PASSWORD'
        }

    //build the container image and push it to the docker hub account

        sh 'docker build -t yanivro/hello-rapid --pull=true .'

        sh 'docker push yanivro/hello-rapid:latest'
//      }

 //   stage 'deploy' {
    //Login to the kubernetes api and run the tunnel to the cluster on localhost:8001 for api calls

      withCredentials([file(credentialsId:	'6b2a4c4f-3265-4e20-93f4-1aa081620e32', variable: 'GOOGLE_SA_KEY')]) {

        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_SA_KEY'

        sh 'gcloud config set container/use_client_certificate True'

        sh 'kubectl proxy --cluster=cluster-1 &'

        sh 'curl http://localhost:8001/api/'

        }
 //     }


}


