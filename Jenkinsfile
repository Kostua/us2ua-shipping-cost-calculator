pipeline {
	tools {
		maven 'maven 3.5.4'
		jdk 'jdk_11'
		}
	agent {
		docker {
			image 'maven:3-alpine'
			args '-v /root/.m2:/root/.m2'
		}
	}
    triggers {
        pollSCM('H/15 * * * *')
    }


	stages {
	    stage ('Initialize') {
		steps {
			sh '''
			   echo "PATH = ${PATH}"
			   echo "M2_HOME = ${M2_HOME}"
			'''
			}
		}
	    stage("Checkout") {
		steps {
			git url: 'https://github.com/Kostua/calculator.git'
		}
	   
	  }
	    stage('SonarQube analysis') {
		   steps { 
			withSonarQubeEnv('sonar'){
		        sh 'mvn clean package sonar:sonar'	
			}
		}
	}

	    stage("Build") {
		steps {
		  sh 'mvn -B -Dmaven.test.failure.ignore=true install' 
     		 }
		post {
		   success {
			junit 'target/surefire-reports/**/*.xml'
			}
		}
	   }



}

}
