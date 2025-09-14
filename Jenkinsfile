pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    
    environment {
        PATH = "/opt/apache-maven-3.9.2/bin:$PATH"
    }
    
    stages {
        stage("Build") {
            steps {
                sh 'mvn clean deploy'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    environment {
                     scannerHome = tool 'congthanh-sonar-scanner'
                    }
                    steps{
                    withSonarQubeEnv('sonarqube-server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
