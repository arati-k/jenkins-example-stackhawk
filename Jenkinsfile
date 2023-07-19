pipeline {
    agent {label 'linux' }

    environment {
        STACKHAWK_API_KEY = credentials("stackhawk-api-key")
    }

    stages {
        stage("Install Docker") {
            steps {
                   sh """ sudo yum update -y
                   sudo yum -y install yum-utils
                   sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
                   sudo yum -y install docker-ce docker-ce-cli containerd.io
                   sudo systemctl start docker
                   sudo systemctl enable docker """
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
