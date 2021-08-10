pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
environment {
         stepLevel = 'start';
        
    }
  
  stages {
  	
    stage('Checkout Code') {
      steps {
        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'github')
        
      }
    }

    stage('mvn build') {
      steps {
        sh '''cd spring-boot-package-war 
			echo $BUILD_ID
			mvn compile'''
        
      }
    }

    stage('test the app') {
      steps {
        sh '''cd spring-boot-package-war 
			mvn test'''
        
      }
    }

    stage('packging') {
      steps {
        sh '''cd spring-boot-package-war 
			mvn clean package & '''
        zip(zipFile: 'package.zip', archive: true, overwrite: true)
        archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
        cleanWs(cleanWhenSuccess: true)
      }
    }
	
  }
  post {
       // only triggered when blue or green sign
       success {
           slackSend(channel: 'david-varshoer', message: 'pipleline ended artifact is ready !', color: '#008000')
       }
       // triggered when red sign
       failure {
	       slackSend(channel: 'david-varshoer', message: " ${stepLevel} FAILED for ${env.JOB_NAME} #${env.BUILD_NUMBER} ", color: '#ff0000', failOnError: false)
       }       
    }
}
