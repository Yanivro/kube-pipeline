#!groovy

node {


    stage 'checkout' {

    git 'https://github.com/docker-training/webapp.git'

    }

    stage 'build' {
    //Authenticate with docker hub in order to push artifact into it

        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker login',
        usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD']]) {

          sh 'sudo docker login --username $DOCKER_USER --password $DOCKER_PASSWORD'
        }

    //build the container image and push it to the docker hub account

        sh 'sudo docker build -t yanivro/hello-world --pull=true .'

        sh 'sudo docker push yanivro/hello-world'
      }

    stage 'deploy' {
    //Login to the kubernetes api and run the tunnle to the cluster on localhost:8001 for api calls

      withCredentials([file(credentialsId:	'google service account json', variable: GOOGLE_SA_KEY)]) {

        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_SA_KEY'

        sh 'kubectl proxy --cluster=cluster-1 &'

        sh 'curl http://localhost:8001/api/'

        }
      }


}

