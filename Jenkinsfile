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
                label 'node_machine'
            }
            steps
            {
                // clone code from a GitHub repository
                git branch: 'Meghana_WS', url: 'https://github.com/MeghanaMahale/DevOpsWorkShopProject-Parent.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean test package"

             }
        }
    }

}
