node('wallmart_node')
{
    
def mavenHome = tool name: "maven3.8.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode')
{
   git branch: 'development', credentialsId: '2ccbe603-4000-4f99-bf57-f914a4dbba13', url: 'https://github.com/harishharishkr/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExcuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployApllicationTomcatServer')
{
sshagent(['04987961-fc2e-4b0a-887d-657d9bc52079']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.173.164:/opt/apache-tomcat-9.0.54/webapps/"
}
}

stage('SendEmailNotification')
{
emailext body: '''Build information regarding project

regards,
hareesh
7337009013''', subject: 'Build info', to: 'hareeshkumar.9356@gmail.com'
}
 }
