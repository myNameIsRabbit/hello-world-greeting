node('master'){
	def mvnHome
	stage('Preparation') { // for display purposes     
      mvnHome = tool 'MAVEN_HOME'
		  checkout scm
			// This can be nexus3 or nexus2
      NEXUS_VERSION = "nexus3"
      // This can be http or https
      NEXUS_PROTOCOL = "http"
      // Where your Nexus is running
      NEXUS_URL = "172.17.0.3:8081"
      // Repository where we will upload the artifact
      NEXUS_REPOSITORY = "repository-example"
      // Jenkins credential id to authenticate to Nexus OSS
      NEXUS_CREDENTIAL_ID = "nexus-credentials"
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
		    
  }  
}
