pipeline {
    agent any
    stages {
        stage("Verify Tooling") {
            steps {
                sh '''
                    docker version
                    docker info
                    docker compose --version
                    curl --version
                    jq --version
                ''' 
            }
        }
        stage('Prune Docker Data') {
            steps {
                sh 'docker system prune -a --volumne -f'
            }
        }
        stage("Start Container") {
            steps {
                sh 'docker-compose up -d --no-color --wait'
                sh 'docker-compose ps'
            }
        }
        stage('Run tests againts container') {
            steps {
                sh 'curl http://localhost:3000/param?query=demo | jq'
            }
        }
    }

    post {
        always {
            sh 'docker-compose down --remove-orphans -v'
            sh 'docker-compose ps'
        }
    }
}