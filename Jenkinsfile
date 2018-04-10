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
   
   stage('Deploy') {
       
        sh 'mv $WORKSPACE/target/*.war /opt/tomcat/webapps/'

  }
  catch (e) {
      currentBuild.result = "FAILED"
      delivery.notifyFailure(e.getMessage())
      throw e
    }

}


