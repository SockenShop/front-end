pipeline {
    environment {
        DOCKER_ID = "m0ph"
        DOCKER_IMAGE_FRONT_END = "front-end"
        DOCKER_TAG = "${BUILD_ID}"
        BUILD_AGENT  = ""
        NAMESPACE = "sock-shop"
    }
agent any
    stages {
        stage('Build') {
            steps { //create a loop somehow??
            sh 'docker build -t $DOCKER_ID/$DOCKER_IMAGE_FRONT_END:$DOCKER_TAG ./microservices/front-end'
                   }
        }
        stage('Run') {
            steps {
                sh 'docker network create $BUILD_TAG'
                sh 'docker run -d --name $DOCKER_IMAGE_FRONT_END --rm --network $BUILD_TAG $DOCKER_ID/$DOCKER_IMAGE_FRONT_END:$DOCKER_TAG'
                sh 'docker stop $DOCKER_IMAGE_FRONT_END'
                sh 'docker network rm $BUILD_TAG'    
                }
        }
        stage('Push') {
            environment {
                DOCKER_PASS = credentials("DOCKER_HUB_PASS")
            }
         steps {
            sh 'docker image tag $DOCKER_ID/$DOCKER_IMAGE_FRONT_END:$DOCKER_TAG $DOCKER_ID/$DOCKER_IMAGE_FRONT_END:latest'
            sh 'docker login -u $DOCKER_ID -p $DOCKER_PASS'
            sh 'docker push $DOCKER_ID/$DOCKER_IMAGE_FRONT_END:$DOCKER_TAG && docker push $DOCKER_ID/$DOCKER_IMAGE_FRONT_END:latest'
            }
        }
        stage('Deploy VPS') {
            environment {
                KUBECONFIG = credentials("VPS_KUBE_CONFIG")
            }
            steps{
                sh 'rm -Rf .kube'
                sh 'mkdir .kube'
                sh 'cat $KUBECONFIG > .kube/config'
                sh 'kubectl apply -f ./manifests -n $NAMESPACE'
                }
            

        }
    }
}
