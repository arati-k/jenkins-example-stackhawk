pipeline {
    agent any

    environment {
        STACKHAWK_API_KEY = credentials("stackhawk-api-key")
    }

    stages {
        stage("Install Docker") {
            steps {
                   sh 'yum -y install yum-utils'
                   sh 'yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo'
                   sh 'yum -y install docker-ce docker-ce-cli containerd.io'
                   sh 'systemctl start docker'
                   sh 'systemctl enable docker'
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
