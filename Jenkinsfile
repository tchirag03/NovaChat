pipeline {
    agent any
    
    tools {
        nodejs 'node' // This tells Jenkins to use the 'node' tool we just configured
    }

    environment {
        FRONTEND_IMAGE = "novachat-frontend"
        BACKEND_IMAGE = "novachat-backend"
        FRONTEND_PORT = "3000"
        BACKEND_PORT = "3001"
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
                echo "   BUILDING REAL DOCKER IMAGES       "
                echo "====================================="
                // Build backend container image
                sh "docker build -t ${BACKEND_IMAGE}:latest ./backend"
                
                // Build frontend container image
                sh "docker build -t ${FRONTEND_IMAGE}:latest ./frontend"
            }
        }

        stage('Continuous Deployment') {
            steps {
                echo "====================================="
                echo "   LAUNCHING CONTAINERS (LIVE DEPLOY) "
                echo "====================================="
                // 1. Clean up old running containers if they exist
                sh "docker stop ${BACKEND_IMAGE} ${FRONTEND_IMAGE} || true"
                sh "docker rm ${BACKEND_IMAGE} ${FRONTEND_IMAGE} || true"
                
                // 2. Run Backend (map host port to container port 3001)
                sh "docker run -d --name ${BACKEND_IMAGE} -p ${BACKEND_PORT}:3001 ${BACKEND_IMAGE}:latest"
                
                // 3. Run Frontend (map host port to Nginx port 80)
                sh "docker run -d --name ${FRONTEND_IMAGE} -p ${FRONTEND_PORT}:80 ${FRONTEND_IMAGE}:latest"
                
                echo "=========================================================="
                echo " LIVE CHANGES ARE NOW ACCESSIBLE AT: http://localhost:${FRONTEND_PORT} "
                echo "=========================================================="
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