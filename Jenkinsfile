pipeline {
    agent any

    // environment {
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
                    echo "==========Scanning the images ======="
                    
                    // Display Trivy version
                    bat 'trivy --version'
                    
                    // List of services to scan
                    def services = ['app', 'mysql']
                    
                    // Loop through services to scan each image
                    for (service in services) {
                        // Retrieve the image ID using docker-compose and store it in a variable
                        def imageId = bat(script: "docker-compose images ${service} -q", returnStdout: true).trim()
                        def imageId_trimmed = imageId.readLines().last().trim()

                        if (imageId_trimmed) {
                            echo "Scanning image for service: ${service} ${imageId_trimmed}"
                            
                            // this line below works  
                            // info it trivy is fixing vuln stuff
                            def scanResult = bat(script: "trivy -q image --light --severity CRITICAL,HIGH ${imageId_trimmed}", returnStdout: true)
                            echo "Scan result for ${service}: ${scanResult}"
                            
                            // Run Trivy scan containers and save report on a file for each image
                            // bat 'trivy -q image --light --severity CRITICAL,HIGH --format json -o "D:\\Desktop\\${service}_scan_report.json" ${imageId_trimmed}'
                        } else {
                            echo "No image found for service: ${service}"
                        }
                    }
                }
            }
        }

    }
    post {
        always {
            // Cleaning up the workspace
            script {
                
                echo "=============turning OFF containers==============="
                bat 'docker-compose down'
                echo '=====LOG====docker-compose-exit-code2: %ERRORLEVEL%'
                
                echo "=============cleaning up the workspace==============="
                bat 'del /q /s * && for /d %%p in (*) do rmdir "%%p" /s /q'
                
                echo "=============removing the .git folder==============="
                bat 'rmdir /s /q .git'
            }
        }
    } 

}