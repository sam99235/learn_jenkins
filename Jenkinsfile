pipeline {
    agent any

    environment {
        COMPOSE_FILE = "compose.yml"
        //trivy , docker , docker-compose
        //question i still need in which env this pipeline will be executed in 
        //answer since it's running this pipeline in my local machine so ir's already pre-installed
        //with *docker
        
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Checking out code...'
                git branch: "main", url: 'https://github.com/sam99235/learn_jenkins.git'
            }
        }

        stage('Run Containers with Docker Compose') {
            steps {
                // Start the services using Docker Compose
                echo "===building and running the containers"
                bat 'cd todo-list-app'
                bat 'docker-compose up -d'
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
        always {
            echo 'Cleaning  up my workspace...'
            // Remove all files in the workspace
            script {
                    bat 'del /q /s * && for /d %%p in (*) do rmdir "%%p" /s /q'
            }
        }

}
