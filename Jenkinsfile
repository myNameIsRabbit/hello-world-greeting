node('master'){
  stage('Poll'){
		checkout scm
	} 
	stage('Build') {
    'mvn clean verfiy -DskipITs=true';
		junit '**/Target/surefire-reports/TEST-*.xml'
		archive 'target/*.jar'
  }
  stage('Results') {
     junit '**/target/surefire-reports/TEST-*.xml'
     archive 'target/*.war'
  }
}
