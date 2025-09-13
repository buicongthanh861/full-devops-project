pieline {
    agent {
        node {
            label 'maven'
        }
    }

    stages {
        stage('Clone-code') {
            steps {
                git branch: 'main', url: 'https://github.com/buicongthanh861/full-devops-project.git'
            }
        }
    }
}