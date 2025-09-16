pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    
    environment {
        PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
    }
    
    stages {
        stage("Build") {
            steps {
                echo "---------- build started --------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "---------- build complted --------"
            }
        }
        stage("test") {
            steps {
                echo "---------- unit test started --------"
                sh 'mvn surefire-report:report'
                echo "----------- unit test Complted--------"
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'congthanh-sonar-scanner'
                    withSonarQubeEnv('congthanh-sonarqube-server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1,unit: 'HOURS') {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
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
