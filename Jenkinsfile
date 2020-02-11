pipeline {
  agent {
    node {
      label 'master'
    }

  }
  options { disableConcurrentBuilds()
	  buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))}
   
   stages {
	   stage("Build WebCalcApi project"){
     steps {
        script {
            bat returnStatus: true, script: 'mvn -f ./pom.xml clean package -Dmaven.test.skip=true && exit %ERRORLEVEL%'
                }
           }}
		   stage("Run Unit Teset Cases"){
     steps {
        script {
            bat returnStatus: true, script: 'mvn -f ./pom.xml clean test && exit %ERRORLEVEL%'
                }
           }}
		   stage("Run Integration Teset Cases"){
     steps {
        script {
            bat returnStatus: true, script: 'mvn -f ./pom.xml clean integration-test && exit %ERRORLEVEL%'
                }
           }}
		   
		   stage("Run Performance Teset Cases"){
     steps {
        script {
            bat returnStatus: true, script: 'mvn -f ./pom.xml verify && exit %ERRORLEVEL%'
			bat returnStatus: true, script: 'mvn -f ./pom.xml surefire-report:report && exit %ERRORLEVEL%'
               }
           }}
    }
  
  post {
   success{
   	//Collecting the required artifacts.
	deploy adapters: [tomcat8(credentialsId: 'TomcatUseInfo', path: '', url: 'http://localhost:9090/')], contextPath: null, onFailure: false, war: 'target/calculator.war'
	archiveArtifacts 'target/calculator.war,target/jmeter/reports/**/*.*,target/site/cobertura/**/*.*,target/site/surefire-report.html'
	//Send the email for build status.
   }
    }
	}



