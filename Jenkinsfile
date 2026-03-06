pipeline {

    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'stage', 'prod'], description: 'Select Environment')
        string(name: 'VERSION', defaultValue: 'v1', description: 'Docker Image Version')
        string(name: 'PORT', defaultValue: '8081', description: 'Application Port')
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/sujanvijay/Parameters.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t sujanapp:${params.VERSION} ."
            }
        }

        stage('Run Container') {
            steps {
                sh """
                docker rm -f sujancontainer || true
                docker run -d -p ${params.PORT}:80 --name sujancontainer sujanapp:${params.VERSION}
                """
            }
        }

        stage('Show Environment') {
            steps {
                echo "Deploying to ${params.ENV}"
                echo "Running on port ${params.PORT}"
                echo "Version ${params.VERSION}"
            }
        }

    }
}
