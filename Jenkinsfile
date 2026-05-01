pipeline {
    agent any

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
    }
}
