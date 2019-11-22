pipeline {
   agent any
tools {
        maven 'maven'
        jdk 'jdk'
		git 'GIT'
    }
     stages {
	 	  
      stage('checkout') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/sushmab123/INGFavAccount.git'
            
            }

             }
		stage("build & SonarQube analysis") {
         steps {
              withSonarQubeEnv('sonar') {
                sh 'mvn -Dmaven.test.failure.ignore=true clean verify sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
		
			 			  stage('Deploy') {
         steps {
            
            // upload artifacts to Nexus.
            sh "mvn -Dmaven.test.failure.ignore=true clean deploy"

            }

             }
			 stage('Release') {
         steps {
            // run the jar file
            sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -jar $WORKSPACE/target/*.jar &'
            }

             }
   }
}
