pipeline {
    agent {
    label "docker-agent"
    }
    stages {
        stage('Verify installation') {
            steps {                
                    sh '''
                    docker --version
                    docker info
                    docker-compose --version
                    curl --version
                    jq --version
                    '''                
            }
        }
        stage('Check container is running or not'){
            steps {
                sh 'docker ps'
                sh 'docker ps -a'
            }
        }

        stage('starting container'){
            steps{
                sh 'docker compose up -d --no-color --wait'
                sh 'docker compose ps'
            }
        }


        stage('Testing the 80 port container'){
            steps{
                sh 'curl http//:192.168.1.13 | jq'
            }        
        }
    }

}
