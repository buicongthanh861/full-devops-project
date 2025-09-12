pipeline {
    agent {
        label 'maven'
    }

    stages {
        stage('Checkout Code') {
            steps{
                //check code tu github
                git branch: 'main', url: 'https://github.com/buicongthanh861/full-devops-project.git'
            }
        }

        stage("Build with Maven") {
            steps {
                //chay maven bang duong dan tuyet doi
                sh '/opt/apache-maven-3.9.9/bin/mvn clean deploy'
            }
        }
    }

    post {
        success {
            echo "Build and deploy thanh cong!!"
        }
        failure {
            echo "Build that bai!"
        }
    }
}