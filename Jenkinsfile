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
           }
		   stage("Run Unit Teset Cases"){
     steps {
        script {
            bat returnStatus: true, script: 'mvn -f ./pom.xml clean test && exit %ERRORLEVEL%'
                }
           }
		   stage("Run Unit Teset Cases"){
     steps {
        script {
            bat returnStatus: true, script: 'mvn -f ./pom.xml clean integration-test && exit %ERRORLEVEL%'
                }
           }
    }
  }
  post {
   success{
   	//Collecting the required artifacts.
	archiveArtifacts '/target/calculatorWeb.war'
	//Send the email for build status.
   }
    }
}