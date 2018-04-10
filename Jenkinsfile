def callS3Upload(mesg){
  stage("${mesg}"){
  withAWS(credentials:'mobile-s3-user', region:'ap-south-1') {
    s3Upload(acl: 'PublicRead', bucket: 'mybucket-ssp', cacheControl: '', excludePathPattern: '', file: "sspWebApp-${env.BUILD_NUMBER}", metadatas: [''], path: "${env.BRANCH_NAME}/sspWebApp-${env.BUILD_NUMBER}")
  }
  }
}


node() {

stage('Retrieve source code') {
    checkout scm
    }


try {
    stage('Maven Build') {
      docker.image('maven:3.5-jdk-8-alpine').inside {
        sh "mvn clean package -Dbuild.number=${BUILD_NUMBER}"
       }   
    }  
    
   stage('Artifactory') {

     callS3Upload("Upload to S3")

   }  
   
   stage('Deploy') {
       
        sh "/bin/mv -f $WORKSPACE/target/*.war /opt/tomcat/webapps/"
    }

  }
  catch (e) {
      currentBuild.result = "FAILED"
      throw e
    }

}


