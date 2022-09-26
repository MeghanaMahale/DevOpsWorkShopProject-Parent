pipeline {
    agent none

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven 3.6.3"
    }

    stages {
        stage('Build') {
            agent 
	    {
                label 'node_machine'
            }
	    steps
	    {
                // clone code from a GitHub repository
                git branch: 'Meghana_WS', url: 'https://github.com/minutuscomputing/DevOpsWorkShopProject-Parent.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean test package"

             }
        }
        stage('Maven-Deploy') {
  	    agent
	    {
                label 'node_machine'
            }
	    steps 
	    {
	        sh 'mvn deploy'
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
            agent
	    {
               label 'ansible'
            }
	    steps 	   
	    {
     	       git branch: 'Neelam_tools', url: 'https://github.com/minutuscomputing/devops-workshop-tools.git'
               sh 'ansible-playbook ./ansible/example/deploy.yml'               
            }         
        }

    }
}
