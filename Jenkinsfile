pipeline {
    agent {
        label 'maven'
    }
    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:/opt/apache-maven-3.9.9/bin:${env.PATH}"
    }
    stages {
        stage('Checkout Code') {
            steps{
                git branch: 'main', url: 'https://github.com/buicongthanh861/full-devops-project.git'
            }
        }

        stage('Check Tools') {
            steps {
                sh 'java -version'
                sh 'ls -la /opt/apache-maven-3.9.9/bin/mvn || echo "Maven not found at specified path"'
                sh 'which mvn || echo "Maven not in PATH"'
            }
        }

        stage("Build with Maven") {
            steps {
                sh 'mvn clean deploy'
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