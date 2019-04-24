node('master'){
	def mvnHome
	stage('Preparation') { // for display purposes     
      mvnHome = tool 'MAVEN_HOME'
   }
  stage('Poll'){
		checkout scm
	} 
	stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.war'
   }
}
