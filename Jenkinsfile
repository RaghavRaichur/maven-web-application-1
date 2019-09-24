node{
//node('master'){

  //http://JenkinsServerIPAddress:8080/pipeline-syntax/globals#currentBuild
  //Getting the  env  global varibale values

  echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
  properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
  ])
  
  def mavenHome=tool name: "mavenv3.6.2", type: "maven"
    
  stage('CheckouttheCode') {
    git branch: 'master',  credentialsId: '809850c3-8f80-4827-80eb-6c039901c5ba', url: 'https://github.com/RaghavRaichur/maven-web-application-1.git'
  
 }
 
  /*
   stage('Checkout'){
     checkout scm
  }
  */
 
 stage('Build')
 {
  sh  "${mavenHome}/bin/mvn clean package"
 }

 /* 
 stage('Testing')
   {
    if(isUnix()){
     sh 'mvn test'
      }
      else{
       bat 'mvn test'   
      }
   }
 */

 /*
 stage('SonarQubeReport')
 {
  sh  "${mavenHome}/bin/mvn sonar:sonar"
 }
*/
  stage('UploadArtifactsIntoNexus')
 {
  sh  "${mavenHome}/bin/mvn deploy"
 }
 /*
 stage('DeplotoTomcat'){
     
     sh "cp $WORKSPACE/target/*.war /opt/apache-tomcat-9.0.16/webapps/"
 }
 */

stage('DeploytoTomcat')
{
sshagent(['c00c8f81-598f-4b27-8a30-711d25bebeaa']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.168.199:/opt/apache-tomcat-9.0.24/webapps/maven-web-application.war"
}
}
 stage('EmailNotification')
  {
    mail to: ragraich@gmail.com,{ 
         subject: 'Build Notification'
         body: '''Build Done, Please check the build log for more details..
         
                  Regards,
                  Raghavendra 
                  }
   }
 
 stage("SlackNotification"){
     slackSend baseUrl: 'https://devops-team-bangalore.slack.com/services/hooks/jenkins-ci/', channel: 'build-notification', message: 'Build done through', tokenCredentialId: '12797dc5-eb70-4f19-8e05-8c07bc58d79d'
 }
}
