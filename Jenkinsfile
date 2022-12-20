pipeline {
    agent any

    tools {
        maven "Default"
    }

    stages {
        stage('Build') {
	    
	    steps
	    {
	        
                git branch: 'master', url: 'https://github.com/MeghanaMahale/DevOpsWorkShopProject-Parent.git'

                
                sh "mvn -Dmaven.test.failure.ignore=true clean package "

             }
             
        }
        
             stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
        stage('SonarQube analysis') {
            environment {
             scannerHome = tool 'sonarqube-4.7.0'
          }
          
          steps{
              withSonarQubeEnv('sonarqube-9.7.1') { 
                sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=demoapp-project \
                   -Dsonar.projectName=demoapp-project \
                   -Dsonar.projectVersion=0.0.1 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/crm/crm/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml"
              }
          }
       }
       
       stage('Quality Gates') {
          steps{
           echo "test1"
				sleep time: 30000, unit: 'MILLISECONDS'
				echo "test2"
				script {
						echo "Waiting for Quality gate"
						def qualitygate = waitForQualityGate()
                        if (qualitygate.status != "OK") {
                           error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
              }
				}
    }
}
    }
}
