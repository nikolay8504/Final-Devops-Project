pipeline {
    agent any
    environment {
        image_name = "312165067517.dkr.ecr.eu-central-1.amazonaws.com/tedoflask"
        region = "eu-central-1"
    }
    stages {
        stage("Build") {
            steps {
                script {
                    
                    sh '''
                    aws ecr get-login-password --region $region | docker login --username AWS --password-stdin 312165067517.dkr.ecr.eu-central-1
                    docker build -t $image_name:latest .
                    '''
                }
            }
        }
        stage("Test") {
            steps {
                script {
                    
                    sh '''
                    curl localhost:$port
                    exit_status=$?

                    if [[ $exit_status == 0 ]]; then
                        echo "SUCCESSFUL TESTS" && docker stop $container_name
                    else
                        echo "FAILED TESTS" && docker stop $container_name && exit 1
                    fi
                    '''
                }
            }
        }
        stage("Push") {
            steps {
                script {
                    
                    sh '''
                    docker push $image_name:latest
                    '''
                }
            }
        }
        stage("Deploy") {
            steps {
                script {
                    
                    sh '''
                    helm upgrade flask helm/ --install --wait --atomic
                    '''
                }
            }
        }
    }
}
