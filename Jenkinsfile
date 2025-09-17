def registry = "https://trial4tof6s.jfrog.io"

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

//        stage('Quality Gate') {
 //           steps {
 //               timeout(time: 1,unit: 'HOURS') {
  //                  script {
  //                      def qg = waitForQualityGate()
  //                      if (qg.status != 'OK') {
  //                          error "Pipeline aborted due to quality gate failure: ${qg.status}"
   //                     }
  //                  }
   //             }
   //         }
   //     }

   
        stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artfiact-cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
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
