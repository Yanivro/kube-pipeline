#!groovy

node {


    stage ('Checkout') {
    //pull from git and Authenticate with docker hub

      git 'https://github.com/Yanivro/rapid-app.git'

       //save commit id to file to access in later stage and remove newline from the file
        sh "git rev-parse --short HEAD > .git/commit-id-temp"
        sh  "tr -d '\n' < .git/commit-id-temp > .git/commit-id"

        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'b7e17834-ab59-4eb3-ad04-b9518cca25a2',
        usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD']]) {

            sh 'docker login --username $DOCKER_USER --password $DOCKER_PASSWORD'
        }
    }


    stage ('Build') {
    //build the container image and push it to the docker hub account with a build_id tag

        //get commit id from file we saved earlier
        COMMIT_ID = readFile('.git/commit-id')

        sh "docker build -t yanivro/hello-rapid:$COMMIT_ID --pull=true . "

        sh "docker push yanivro/hello-rapid:$COMMIT_ID"
     }


    stage ('Deploy') {
    //Login to the kubernetes api, set the correct cluster context and replace current app image.

      withCredentials([file(credentialsId:	'6b2a4c4f-3265-4e20-93f4-1aa081620e32', variable: 'GOOGLE_SA_KEY')]) {

        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_SA_KEY'

        //get commit id from file we saved earlier
        COMMIT_ID = readFile('.git/commit-id')

        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {

            sh 'kubectl config use-context gke_smooth-league-152210_europe-west1-c_cluster-1'

            sh 'kubectl config current-context'

            sh "kubectl set image deployment/app app=yanivro/hello-rapid:$COMMIT_ID --namespace=app"
        }
      }
    }
}


