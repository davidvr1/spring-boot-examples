pipeline {
  agent {
    node {
      label 'centos7'
    }
  }
  
  stages {
		stepLevel = 'start';
	
		stage('Checkout Code') {
		  steps {
			try{
				stepLevel = 'Checkout Code'
				git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'github')
			} catch (e) {
				slackSend(channel: 'david-varshoer', message: "FAILED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}), color: ''#ff0000'')
				sh 'exit 1'
			}				
		  }
		}
		
		stage('mvn build') {
		  steps {
			try{
				stepLevel = 'mvn build'
				sh '''cd spring-boot-package-war 
						echo $BUILD_ID
						mvn compile'''
			} catch(e) {			
				slackSend(channel: 'david-varshoer', message: 'mvn build ${env.BUILD_NUMBER} failed!', color: '#ff0000', failOnError: false)
				sh 'exit 1'
				throw e;
			}
		  }
		}

		stage('test the app') {
		  steps {
			try{
				stepLevel = 'test the app'
				sh '''cd spring-boot-package-war 
					  mvn test'''
			} catch(e) {
				sh 'exit 1'
				slackSend(channel: 'david-varshoer', message: 'test for ${env.JOB_NAME} #${env.BUILD_NUMBER} failed', color: '#ff0000', failOnError: false)
				throw e;
			}
		  }
		}

		stage('packging') {
		  steps {
			try{
				stepLevel = 'packging'
				sh '''cd spring-boot-package-war 
				mvn clean package & '''
				zip(zipFile: 'package.zip', archive: true, overwrite: true)
				archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
				cleanWs(cleanWhenSuccess: true)
				slackSend(channel: 'david-varshoer', message: 'pipleline ended artifact is ready !', color: '#008000')
			} catch(e) {
				sh 'exit 1'
				slackSend(channel: 'david-varshoer', message: 'packging for ${env.JOB_NAME} #${env.BUILD_NUMBER} failed', color: '#ff0000', failOnError: false)
				throw e;
			}
		  }
		}
		
  }
}
