pipeline {
  agent {
    node {
      label 'master'
    }

  }
  options { disableConcurrentBuilds()
	  buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))}
   
   stages {
	   stage("Build WebCalcAppi project"){
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
    }
  
  post {
   success{
   	//Collecting the required artifacts.
	deploy adapters: [tomcat8(credentialsId: 'TomcatUseInfo', path: '', url: 'http://localhost:9090/')], contextPath: null, onFailure: false, war: 'target/calculatorWeb.war'
	archiveArtifacts 'target/calculatorWeb.war'
	//Send the email for build status.
   }
    }
	}