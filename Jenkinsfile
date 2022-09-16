pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven 3.8.6"
    }
    
    stages {
        stage('SCM Checkout') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/LoksaiETA/Java-mvn-app2.git'
            }
		}
        stage('Maven Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
		}
        stage('Approval Step'){
            steps{
                
                //----------------send an approval prompt-------------
                script {
                   env.APPROVED_DEPLOY = input message: 'User input required Choose "yes" | "Abort"'
                       }
                //-----------------end approval prompt------------
            }
        }
        stage('Deploy to QA AppServer') {
            steps {
				script {
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'AppServer', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: 'target/', sourceFiles: 'target/mvn-hello-world.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])				}
            }
		}
	}
}
