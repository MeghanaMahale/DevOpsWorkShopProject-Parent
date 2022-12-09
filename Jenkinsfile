pipeline {
    agent none

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "3.6.3"
    }

    stages {
        stage('Build') {
            agent 
	    {
                label 'node'
            }
	    steps
	    {
	        //def mavenPom = readMavenPom 'pom.xml'
                // clone code from a GitHub repository
                git branch: 'master', url: 'https://github.com/MeghanaMahale/DevOpsWorkShopProject-Parent.git'

                // Run Maven on a Unix agent.
               
                sh "mvn -Dmaven.test.failure.ignore=true -Djob_name=${JOB_NAME} -Dv=${BUILD_NUMBER} clean test package deploy "

             }
        }
        stage('Maven-Deploy') {
  	    agent
	    {
                label 'node'
            }
	    steps 
	    {
	        sh 'mvn deploy'
   	    }    	    
            post
	    {
            
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
     	       git branch: 'master', url: 'https://github.com/MeghanaMahale/deploy_playbook.git'
               sh " ansible-playbook -i inventory test.yml --extra-vars 'artifact_id=${env.JOB_NAME}'"               
            }  
       }
    }
}
