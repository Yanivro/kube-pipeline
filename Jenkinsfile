#!groovy

node {


    stage ('Checkout') {
      git 'https://github.com/Yanivro/rapid-app.git'

    //Authenticate with docker hub in order to push artifact into it

        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '459bf397-3910-4c22-8d0b-55107eadcbb5',
        usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD']]) {

          sh 'docker login --username $DOCKER_USER --password $DOCKER_PASSWORD'
        }
    }

    stage ('build') {
    //build the container image and push it to the docker hub account

        sh 'docker build -t yanivro/hello-rapid:$BUILD_ID --pull=true .'

        sh 'docker push yanivro/hello-rapid:$BUILD_ID'
     }


    stage ('Deploy') {
    //Login to the kubernetes api and run the tunnel to the cluster on localhost:8001 for api calls

      withCredentials([file(credentialsId:	'6b2a4c4f-3265-4e20-93f4-1aa081620e32', variable: 'GOOGLE_SA_KEY')]) {

        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_SA_KEY'

        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {

            sh 'kubectl config use-context gke_smooth-league-152210_europe-west1-c_cluster-1'

            sh 'kubectl config current-context'

            sh 'kubectl set image deployment/app app=yanivro/hello-rapid:$BUILD_ID --namespace=app'
        }
      }
    }
}


