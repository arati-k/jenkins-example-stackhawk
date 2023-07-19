pipeline {
    agent any

    environment {
        STACKHAWK_API_KEY = credentials("stackhawk-api-key")
    }

    stages {
        stage("Install Docker") {
            steps {
                   sh '''yum update -y'''
            }
        }

        stage("Deploy site") {
            steps {
                sh 'sudo cp index.json /var/www/html'
            }
        }

        stage("Run HawkScan Test") {
            steps {
                sh """
                    sudo docker run -v ${WORKSPACE}:/hawk:rw -t \
                        -e API_KEY=${STACKHAWK_API_KEY} \
                        -e NO_COLOR=true \
                        stackhawk/hawkscan
                """
            }
        }
    }
}
