node('master'){
	stage('Poll'){
		checkout scm
	}
	stage('Build & Unit test'){
		withMaven(maven: 'MAVEN_HOME'){
			'mvn clean verfiy -DskipITs=true';          
		}
	}
	stage('Results') {
      		junit '**/target/surefire-reports/TEST-*.xml'
      		archive 'target/*.war'
   	}
	stage('Static Code Analysis'){
		'mvn clean verfiy sonar:sonar -Dsonar.projectName=example-project -Dsonar.projectKey=example-project -Dsonar.projectVersion=$NUILD_NUMBER';
	}
	stage('Publish'){
		def server = Artifactory.server 'Default Artifactory Server'
		def uploadSpec = """{
			"files": [
				{
					"pattern":"target/hello-0.0.1.war",
					"target":"example-projec/${BUILD_NUMBER}/",
					"props":"Integration-Tested=Yes;Performance-Tested=No"
				}
			]
		}"""
		server.upload(uploadSpec)
	}
}
