
node
{
    def mavenHome = tool name: "maven3.6.3"
    properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    stage('CheckOutCode')
    {
        git branch: 'development', credentialsId: '0a9bda6e-063d-4dd8-8510-1f76bd8e2ea1', url: 'https://github.com/RefData-IB/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('GenerateReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactsTOnexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('DeployApp')
    {
        sshagent(['4c2d2a91-d7a4-44bf-8195-795a0f296b87']) 
        {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@18.219.123.56:/opt/apache-tomcat-9.0.40/webapps/"
        }
    }
   /* stage('SendEmail')
    {
        mail bcc: '', body: 'Build', cc: 'subhashjob2020@gmail.com', from: '', replyTo: '', subject: 'Build', to: 'subashpradhan2014@gmail.com'
    } */
}
