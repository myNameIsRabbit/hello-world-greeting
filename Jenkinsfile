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
		bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify sonar:sonar -Dsonar.projectName=example-project -Dsonar.projectKey=example-project -Dsonar.projectVersion=$BUILD_NUMBER';/)
	}
	stage('Integration Test'){
		bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify -Dsurefire.skip=true';/)
		junit '**/target/surefire-reports/TEST-*.xml'
    archiveArtifacts 'target/*.war'
	}
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.war'
   }
}
