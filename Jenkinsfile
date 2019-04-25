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
		// Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
    pom = readMavenPom file: "pom.xml";
    // Find built artifact under target folder
    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
    // Print some info from the artifact found
    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
    // Extract the path from the File found
    artifactPath = filesByGlob[0].path;
    // Assign to a boolean response verifying If the artifact name exists
    artifactExists = fileExists artifactPath;	    
  }  
}
