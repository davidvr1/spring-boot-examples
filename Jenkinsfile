pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('Checkout Code') {
      steps {
        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', poll: true, changelog: true, credentialsId: 'github')
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
        sh 'cd spring-boot-package-war mvn clean package &'
        zip(zipFile: 'package.zip', overwrite: true)
        archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
        cleanWs(cleanWhenSuccess: true)
      }
    }

  }
  environment {
    foo = 'nothing'
  }
}