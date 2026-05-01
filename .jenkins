pipeline {
    agent any // Runs on any available Jenkins node

    stages {
        stage('Pull Code') {
            steps {
                // This checks out the code from your GitHub repo
                checkout scm
            }
        }
        
        stage('Show Updates') {
            steps {
                script {
                    // Grab the list of changes in this specific build
                    def changeLogSets = currentBuild.changeSets
                    
                    if (changeLogSets.isEmpty()) {
                        echo "No new commits. This build might have been triggered manually."
                    } else {
                        echo "=== UPDATES IN THIS BUILD ==="
                        for (int i = 0; i < changeLogSets.size(); i++) {
                            def entries = changeLogSets[i].items
                            for (int j = 0; j < entries.length; j++) {
                                def entry = entries[j]
                                // Print the commit hash, message, and author to the console
                                echo "- Commit ${entry.commitId.take(7)}: ${entry.msg} (by ${entry.author})"
                            }
                        }
                        echo "============================="
                    }
                }
            }
        }

        stage('Build and Test') {
            steps {
                echo "Here is where you compile your code or run tests!"
                // Example: sh 'npm install' or sh 'make build'
            }
        }
    }
}
