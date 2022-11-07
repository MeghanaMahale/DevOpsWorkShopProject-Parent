pipeline {
    agent none

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "3.6.3"
    }

    stages {
        stage('Build and deploy') {
            agent any
		
	    steps
	    {
		echo "Build and deploy stage"
                // clone code from a GitHub repository
                git branch: 'Meghana_WS', url: 'https://github.com/minutuscomputing/DevOpsWorkShopProject-Parent.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true -Djob_name=${JOB_NAME} -Dv=${BUILD_NUMBER} clean test package deploy"
                //sh "mvn deploy"
             }
	     post
	     {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
              }
        }

        stage('Server-Deploy') {
          agent any
	    steps 	   
	    {
	       echo "server deploy stage"
     	       git branch: 'Meghana_tools', url: 'https://github.com/minutuscomputing/devops-workshop-tools.git', credentialsId: ''
	       sh 'ansible-galaxy install geerlingguy.java'
                sh "ansible-playbook ./ansible/example/deploy.yml --extra-vars 'artifact_id=${env.JOB_NAME}' "
            
	    }         
        }

    }
}
