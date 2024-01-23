pipeline {
    agent any
    stages {
        stage('build-docker-image') {
            steps {
                build_and_push_docker_image()
            }
        }
        stage('deploying-to-dev') {
            steps {
                deploy("dev")
            }
        }
        stage('testing-on-dev') {
            steps {
                run_api_tests("dev")
            }
        }
        stage('deploying-to-stg') {
            steps {
                deploy("stg")
            }
        }
        stage('testing-on-stg') {
            steps {
                run_api_tests("stg")
            }
        }
        stage('deploying-on-prod') {
            steps {
                deploy("prod")
            }
        }
        stage('testing-on-prod') {
            steps {
                run_api_tests("prod")
            }
        }
    }
}

def build_and_push_docker_image(){
    echo "Building docker image"
    sh "docker build --no-cache -t romansasacovs/python-greetings-app:latest ."

    echo "Pushing docker image"
    sh "docker push romansasacovs/python-greetings-app:latest"
}

def deploy(String environment){
    echo "Deploying Python microservice to ${environment}"
    sh "docker pull romansasacovs/python-greetings-app:latest"
    sh "docker-compose stop greetings-app-${environment}"
    sh "docker-compose rm greetings-app-${environment}"
    sh "docker-compose up -d greetings-app-${environment}"
}

def run_api_tests(String environment){
    echo "API tests on ${environment}"
    sh "docker pull romansasacovs/api-tests:latest"
    sh "docker run --network=host --rm romansasacovs/api-tests:latest run greetings greetings_${$environment}"
}