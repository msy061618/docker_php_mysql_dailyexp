pipeline {
    agent any

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

        stage('Identify and Stop Container') {
            steps {
                script {
                    // Get a list of all running containers
                    def runningContainers = sh(script: "docker ps --format '{{.Names}}'", returnStdout: true).trim()

                    // Check if there are any running containers
                    if (runningContainers) {
                        // Assume the first running container is the one you are looking for
                        def targetContainer = runningContainers.tokenize('\n')[0].trim()

                        echo "Found a running container: ${targetContainer}"

                        // Stop the identified container
                        echo "Stopping the container..."
                        sh "docker stop ${targetContainer}"
                    } else {
                        echo "No running containers found. Skipping stop command."
                    }
                }
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
                sh 'curl http//:192.168.1.17 | jq'
            }        
        }
    }

}
