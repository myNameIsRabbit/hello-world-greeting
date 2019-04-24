node('master'){
	def mvnHome
	stage('Preparation') { // for display purposes     
      mvnHome = tool 'MAVEN_HOME'
		  checkout scm
   }
	stage('Build') {
      // Run the maven build
   	bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/) 
   }
	stage('Static Code Analysis'){
		bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify sonar:sonar -Dsonar.projectName=example-project -Dsonar.projectKey=example-project -Dsonar.projectVersion=$NUILD_NUMBER';/)
	}
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.war'
   }
}
