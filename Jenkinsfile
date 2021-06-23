node{

def mavenHome = tool name: "maven3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode'){
 git branch: 'development', credentialsId: 'a7f8b78d-9d43-44fc-8d89-baef34732940', url: 'https://github.com/prathaptechnologies-ec-apps/maven-web-application.git'

}
stage('Build'){
 sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('Upload ArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoServer'){

sshagent(['5929a976-bcbb-4249-b3d9-f7ba2d721cde']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.82.32:/opt/apache-tomcat-8.5.68/webapps/"
}

}

stage('SendEmailNotification'){
emailext body: '''Build is Over!!

Regards 
Prathap Technologies''', subject: 'Build is Over!!', to: 'k.v.reddyprathap00@gmail.com'

}
}
