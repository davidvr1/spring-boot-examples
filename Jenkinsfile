pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('Checkout Code') {
      steps {
        script {
          env.stepLevel = 'Checkout Code'
        }

        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'github')
      }
    }

    stage('mvn build') {
      steps {
        script {
          env.stepLevel = 'mvn build'
        }

        sh '''cd spring-boot-package-war 
echo $BUILD_ID
mvn compile'''
      }
    }

    stage('test the app') {
      steps {
        script {
          env.stepLevel = 'test the app'
        }

        sh '''cd spring-boot-package-war 
mvn test'''
      }
    }

    stage('packging') {
      steps {
        script {
          env.stepLevel = 'packging'
        }

        sh 'cd spring-boot-package-war mvn clean package & '
        zip(zipFile: 'package.zip', archive: true, overwrite: true)
        archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
        cleanWs(cleanWhenSuccess: true)
      }
    }

  }
  environment {
    foo = 'nothing'
  }
  post {
    success {
      slackSend(channel: 'david-varshoer', message: "pipleline build #${env.BUILD_NUMBER} ended artifact is ready (${env.BUILD_URL})", color: '#008000')
    }

    failure {
      slackSend(channel: 'david-varshoer', message: " ${stepLevel} FAILED for build #${env.BUILD_NUMBER} ", color: '#ff0000', failOnError: false)
    }

  }
}