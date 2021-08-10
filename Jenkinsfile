pipeline {
  agent {
    node {
      label 'centos7'
    }
  }
  try{
  stages {

	
		stage('Checkout Code') {
		  steps {
		    stepLevel = 'Checkout Code'
			git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'github')		
		  }
		}
		
		stage('mvn build') {
		  steps {
			stepLevel = 'mvn build'
			sh '''cd spring-boot-package-war 
					echo $BUILD_ID
					mvn compile'''
			slackSend(channel: 'david-varshoer', message: 'mvn build ${env.BUILD_NUMBER} failed!', color: '#ff0000', failOnError: false)
		  }
		}

		stage('test the app') {
		  steps {
			stepLevel = 'test the app'
			sh '''cd spring-boot-package-war 
				  mvn test'''
			slackSend(channel: 'david-varshoer', message: 'test for ${env.JOB_NAME} #${env.BUILD_NUMBER}  failed', color: '#ff0000', failOnError: false)
		  }
		}

		stage('packging') {
		  steps {
			stepLevel = 'packging'
			sh '''cd spring-boot-package-war 
			mvn clean package & '''
			zip(zipFile: 'package.zip', archive: true, overwrite: true)
			archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
			cleanWs(cleanWhenSuccess: true)
			slackSend(channel: 'david-varshoer', message: 'pipleline ended artifact is ready !', color: '#008000')
		  }
		}
  }
	} catch(e){
			slackSend(channel: 'david-varshoer', message: "FAILED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}), color: ''#ff0000'')
			sh 'exit 1'
	}
  
}
