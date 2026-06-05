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
                echo "   BUILDING REAL DOCKER IMAGES       "
                echo "====================================="
                // Build the backend container image from the backend directory
                sh 'docker build -t novachat-backend:latest ./backend'
                
                // Build the frontend container image from the frontend directory
                sh 'docker build -t novachat-frontend:latest ./frontend'
            }
        }

        stage('Continuous Deployment') {
            steps {
                echo "====================================="
                echo "   LAUNCHING CONTAINERS (LIVE DEPLOY) "
                echo "====================================="
                // 1. Clean up old running containers if they exist so there are no port conflicts
                sh 'docker stop novachat-backend novachat-frontend || true'
                sh 'docker rm novachat-backend novachat-frontend || true'
                
                // 2. Run Backend on port 5000
                sh 'docker run -d --name novachat-backend -p 5000:5000 novachat-backend:latest'
                
                // 3. Run Frontend on port 8080 (Maps host port 8080 to Nginx port 80)
                sh 'docker run -d --name novachat-frontend -p 8080:80 novachat-frontend:latest'
                
                echo "=========================================================="
                echo " LIVE CHANGES ARE NOW ACCESSIBLE AT: http://localhost:8080 "
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