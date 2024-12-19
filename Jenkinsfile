pipeline {
    agent any

    environment {
        COMPOSE_FILE = "compose.yml"
        //i still need in which env this pipeline will be executed in 
        //trivy , docker , docker-compose
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Akaunting repository
                git branch: 'master', url: 'https://github.com/dockersamples/todo-list-app.git'
            }
        }

        stage('Run Containers with Docker Compose') {
            steps {
                // Start the services using Docker Compose
                sh 'cd todo-list-app'
                sh 'docker-compose up -d'
            }
        }

        // stage('Scan Containers with Trivy') {
        //     steps {
        //         // Scan the running containers with Trivy
        //         script {
        //             def services = ['app', 'mysql'] // Define the services to scan
        //             for (service in services) {
        //                 sh "trivy image $(docker-compose images ${service} -q) onvert --format table --severity CRITICAL,HIGH output.json"
        //             }
        //         }
        //     }
        // }
    }
    post {
        always {
            // Clean up Docker Compose services after the pipeline
            sh 'docker-compose down'
        }
    }
}
