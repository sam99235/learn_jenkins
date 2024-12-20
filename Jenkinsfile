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
                    
                    // Display Trivy version
                    bat 'trivy --version'
                    
                    // List of services to scan
                    def services = ['app', 'mysql']
                    
                    // Loop through services to scan each container
                    for (service in services) {
                        // Retrieve the image ID using docker-compose and store it in a variable
                        def imageId = bat(script: "docker-compose images ${service} -q", returnStdout: true).trim()
                        echo '=====executing echo imageid'
                        bat "echo ${imageId}"
                        // if (imageId) {
                        //     echo 'Scanning image for service: ${service} ${imageId}'
                        //     def scanResult = bat(script: "trivy image --light --severity CRITICAL,HIGH ${imageId}", returnStdout: true).trim()
                        //     echo 'Scan result for ${service}: ${scanResult}'
                        //     // Run Trivy scan for the image
                        //     // bat 'trivy -q image --light --severity CRITICAL,HIGH --format json -o ${service}_scan_report.json ${imageId}'
                        //     // bat 'trivy -q image --light --severity CRITICAL,HIGH --format json -o ${service}_scan_report.json ${imageId}'
                        // } else {
                        //     echo 'No image found for service: ${service}'
                        // }
                    }
                }
            }
        }
        // stage('Scan Containers with Trivy') {
        //     steps {
        //         script {
        //             echo "==========Scanning the running containers======="
        //             bat 'trivy --version'
        //             def services = ['app', 'mysql'] // defined services
        //             for (service in services) {
        //                 // current issue is here
        //                 bat 'trivy image $(docker-compose images ${service} -q)' //convert --format table --severity CRITICAL,HIGH output.json"
        //            }
        //             echo  'exit-code3   %ERRORLEVEL%'

        //         }
        //     }
        // }
    }
    post {
        always {
            // Clean up Docker Compose services after the pipeline
            script {
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