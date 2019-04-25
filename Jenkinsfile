node('master'){
	def mvnHome
	stage('Preparation') { // for display purposes     
      mvnHome = tool 'MAVEN_HOME'
		  checkout scm
   }
	stage('Build & Unit Test') {
      // Run the maven build
   	bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/) 
		junit '**/target/surefire-reports/TEST-*.xml'
    archiveArtifacts 'target/*.war'
   }
	stage('Static Code Analysis'){
		bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify sonar:sonar -Dsonar.projectName=example-project -Dsonar.projectKey=example-project -Dsonar.projectVersion=$BUILD_NUMBER';/)
	}
	stage('Integration Test'){
		bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify -Dsurefire.skip=true';/)
		junit '**/target/surefire-reports/TEST-*.xml'
    archiveArtifacts 'target/*.war'
	}
   stage('Publish') {
	   nexusVersion('nexus3')
		 protocol('http')
		 nexusUrl('localhost:8081')
		 groupID('sp.sd')
		 version('0.0.1')
		 repository('repository-example')
		 credentialsId('nexus-credentials')
		 artifact{
			 artifactId('Test')
       type('war')
       classifier('debug')
       file('target/hello-0.0.1.war')
		 }
   }  
}
