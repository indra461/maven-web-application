node
{

def mavenHome= tool name : 'maven3.6.3'

stage('Checkout Code')
{
git branch: 'development', credentialsId: 'e6c304c2-1006-4441-8402-4003ce64e9af', url: 'https://github.com/indra461/maven-web-application.git'
}

stage ('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}


stage ('SonarQubeReportEecution')
{
sh "${mavenHome}/bin/mvn clean sonar:sonar" 
}

stage ('UploadArtifactsintoNexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployApplicationtoTomcat')
{
sshagent(['683865cb-eb3f-45e0-83f4-2056894fc789']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.183.242:/opt/apache-tomcat-9.0.34/webapps/"
}
}

stage('Email')
{
emailext body: '''

Indra''', subject: 'Build over', to: 'indra461.reddy@gmail.com'
}


}
