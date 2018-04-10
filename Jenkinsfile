node() {

stage('Retrieve source code') {
    checkout scm
    }


try {
    stage('Maven Build') {
      docker.image('maven:3.5-jdk-8-alpine').inside {
        sh "mvn clean package"
       }   
    }  
  }
  catch (e) {
      currentBuild.result = "FAILED"
      delivery.notifyFailure(e.getMessage())
      throw e
    }
    finally {
      junit 'target/surefire-reports/*.xml'
    }

}


