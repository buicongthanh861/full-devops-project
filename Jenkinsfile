pipeline {
    agent {
        node {
            label 'maven'
        }
    }
enviroment {
    PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
}

    stages {
        stage('Clone-code') {
            steps {
                git branch: 'main', url: 'https://github.com/buicongthanh861/full-devops-project.git'
            }
        }
    }
}