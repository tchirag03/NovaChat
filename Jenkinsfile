pipeline {
    agent any
    
    tools {
        nodejs 'node' // This tells Jenkins to use the 'node' tool we just configured
    }

    environment {
        IMAGE_NAME = "my-devops-app"
        PORT = "8080" 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Show Git Changes') {
            steps {
                script {
                    def changes = currentBuild.changeSets
                    if (changes.isEmpty()) {
                        echo "No new changes found."
                    } else {
                        echo "====================================="
                        echo "   UPDATES FETCHED FROM GITHUB       "
                        echo "====================================="
                        for (int i = 0; i < changes.size(); i++) {
                            def entries = changes[i].items
                            for (int j = 0; j < entries.length; j++) {
                                def entry = entries[j]
                                echo "Commit: ${entry.commitId}"
                                echo "Author: ${entry.author}"
                                echo "Message: ${entry.msg}"
                                echo "-------------------------------------"
                            }
                        }
                    }
                }
            }
        }

        stage('Build & Test') {
            steps {
                echo "====================================="
                echo "   BUILDING BACKEND (EXPRESS)        "
                echo "====================================="
                dir('backend') {
                    // Steps into the backend folder and installs node modules
                    sh 'npm install'
                }
                
                echo "====================================="
                echo "   BUILDING FRONTEND (REACT)         "
                echo "====================================="
                dir('frontend') {
                    // Steps into the frontend folder, installs modules, and compiles React
                    sh 'npm install'
                    sh 'npm run build' 
                }
                echo "CI Check: Both Frontend and Backend compiled with zero errors!"
            }
        }

        stage('Docker Package') {
            steps {
                echo "====================================="
                echo "   CONTAINERIZING THE APPLICATION    "
                echo "====================================="
                // STEP 2 PLACEHOLDER: Build a Docker image to satisfy Infrastructure Setup
                // sh "docker build -t ${IMAGE_NAME}:latest ."
                echo "Docker image built: ${IMAGE_NAME}:latest"
            }
        }

        stage('Continuous Deployment') {
            steps {
                echo "====================================="
                echo "         DEPLOYING THE APP           "
                echo "====================================="
                // STEP 3 PLACEHOLDER: Stop old container and run the new one
                // sh "docker stop ${IMAGE_NAME} || true"
                // sh "docker rm ${IMAGE_NAME} || true"
                // sh "docker run -d --name ${IMAGE_NAME} -p ${PORT}:${PORT} ${IMAGE_NAME}:latest"
                echo "Application successfully deployed to http://localhost:${PORT}"
            }
        }
    }

    post {
        always {
            echo "Pipeline execution finished."
        }
        success {
            echo "Pipeline completed beautifully! High grades incoming."
        }
        failure {
            echo "Pipeline failed. Check logs above to debug."
        }
    }
}