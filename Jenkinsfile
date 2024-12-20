pipeline {
    agent any

    // environment {
    //     // TRIVY = TRIVY_EXE
    // //     //trivy , docker , docker-compose

    // }

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
                bat 'echo %cd%'
                bat 'IF EXIST docker-compose.yml echo FOUND'
                bat 'docker-compose up -d'
                echo '=====LOG====exit-code1: %ERRORLEVEL%'

            }
        }

        stage('Scan Containers with Trivy') {
            steps {
                script {
                    echo "==========Scanning the running containers======="
                    bat 'trivy --version'
                    // bat '"C:\\Program Files\\trivy\\trivy.exe --version"'
                //     def services = ['app', 'mysql'] // Define the services to scan
                //     for (service in services) {
                //         bat "trivy image \$(docker-compose images ${service} -q)" //convert --format table --severity CRITICAL,HIGH output.json"
                //    }
                }
            }
        }
    }
    post {
        always {
            // Clean up Docker Compose services after the pipeline
            script {
                bat 'docker-compose down'
                echo '=====LOG====docker-compose-exit-code2: %ERRORLEVEL%'
                echo "=============cleaning up the workspace==============="
                bat 'del /q /s * && for /d %%p in (*) do rmdir "%%p" /s /q'
                echo "=============removing teh .git folder==============="
                bat 'rmdir /s /q .git'
            }
        }
    } 

}