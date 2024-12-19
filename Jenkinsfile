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
                script {
                    echo '===========Checking out code================='
                    git url: 'https://github.com/sam99235/learn_jenkins.git', branch: "main"
                    
                    //force building now 
                    //checkout scm: [$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], userRemoteConfigs: [[url: 'https://github.com/your-repo.git']]]                
                }
            }
        }

        stage('Run Containers with Docker Compose') {
            steps {
                // Start the services using Docker Compose
                echo "==========building and running the containers======="
                bat 'cwd %cd%'
                bat 'IF EXIST compose.yml echo FOUND'
                bat 'docker-compose up -d'
                echo 'exit-code1: %ERRORLEVEL%'

            }
        }

        // stage('Scan Containers with Trivy') {
        //     steps {
        //         // Scan the running containers with Trivy
        //         script {
        //             def services = ['app', 'mysql'] // Define the services to scan
        //             for (service in services) {
        //                 sh "trivy image $(docker-compose images ${service} -q) onvert --format table --severity CRITICAL,HIGH output.json"
        //            }
        //         }
        //     }
        // }
    }
    post {
        always {
            // Clean up Docker Compose services after the pipeline
            script {
                bat 'docker-compose down'
                echo 'exit-code2: %ERRORLEVEL%'
                bat 'del /q /s * && for /d %%p in (*) do rmdir "%%p" /s /q'
            }
        }
    } 
    // post{
    //     always {
    //         echo 'Cleaning  up my workspace...'
    //         // Remove all files in the workspace
    //         script {
    //                 bat 'del /q /s * && for /d %%p in (*) do rmdir "%%p" /s /q'
    //         }
    //     }
    // }

}
