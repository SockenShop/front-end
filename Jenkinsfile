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
            sh 'docker build -t $DOCKER_ID/$DOCKER_IMAGE_FRONT_END:$DOCKER_TAG .'
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
        stage('Deploy EKS') {
            environment {
                KUBECONFIG = credentials("EKS_CONFIG")  
                AWSKEY = credentials("AWS_KEY")
                AWSSECRETKEY = credentials("AWS_SECRET_KEY")

            }
            steps{
                sh 'rm -Rf .kube'
                sh 'mkdir .kube'
                sh 'cat $KUBECONFIG > .kube/config'
                sh 'rm -Rf .aws'
                sh 'mkdir .aws'
                sh 'sudo aws configure set aws_access_key_id $AWSKEY'
                sh 'sudo aws configure set aws_secret_access_key $AWSSECRETKEY'
                sh 'sudo aws eks --region eu-west-3 update-kubeconfig --name sock-shop-9sQCAT9F'
                sh 'kubectl apply -f ./manifests -n $NAMESPACE'
                }
            

        }
    }
}
