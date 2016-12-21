#!groovy

node {


    stage ('Checkout') {
    //pull from git and Authenticate with docker hub

      git 'https://github.com/Yanivro/rapid-app.git'
      sh "git rev-parse --short HEAD"
      commit_id = readFile('.git/commit-id')
      sh 'echo $commit_id'
      sh 'echo ${commit_id}'
      sh 'echo env.commit_id'
      withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '459bf397-3910-4c22-8d0b-55107eadcbb5',
      usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD']]) {

        sh 'docker login --username $DOCKER_USER --password $DOCKER_PASSWORD'
        }
    }


    stage ('Build') {
    //build the container image and push it to the docker hub account with a build_id tag

        sh 'docker build -t yanivro/hello-rapid:$BUILD_ID --pull=true .'

        sh 'docker push yanivro/hello-rapid:$BUILD_ID'
     }


    stage ('Deploy') {
    //Login to the kubernetes api, set the correct cluster context and replace current app image.

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


