#!groovy

node('node') {


    stage 'Checkout'

    git 'https://github.com/docker-training/webapp.git'


    stage 'Login to repo'
    //Authenticate with docker hub in order to push artifact into it
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker login',
        usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD']]) {

        sh 'sudo docker login --username $DOCKER_USER --password $DOCKER_PASSWORD'
        }

    stage 'Build and push'
    //build the container image and push it to the docker hub account

        sh 'sudo docker build -t yanivro/hello-world:${GIT_COMMIT} --pull=true .'


    stage 'Login to kubernetes api'



    stage 'Deploy'






}


