
node() {

stage('Retrieve source code') {
    checkout scm
    delivery = load 'repository.groovy'
    }
try {
    stage('Maven Build') {
      docker.image('maven:3.5-jdk-8-alpine').inside {
        sh "mvn clean package -Dbuild.number=${BUILD_NUMBER}"
        sh "/bin/mv -f $WORKSPACE/target/*.war $WORKSPACE/Build-${env.BUILD_NUMBER}/sspWebApp_${env.BRANCH_NAME}${env.BUILD_NUMBER}.war"
       }
    }
   stage('Artifactory') {
        delivery.artifactory()
      }
   stage('Deploy') {
        sh "/bin/mv -f $WORKSPACE/Build-${env.BUILD_NUMBER}/sspWebApp_${env.BRANCH_NAME}${env.BUILD_NUMBER}.war /opt/tomcat/webapps/"
    }
  }
  catch (e) {
      currentBuild.result = "FAILED"
      throw e
    }
}

