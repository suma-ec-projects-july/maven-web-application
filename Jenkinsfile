node
{
  def mavenhome = tool name : "maven3.8.2" 
  
 stage('checkoutcode')
   {
       git branch: 'development', credentialsId: '178b60f4-b6a2-4c4c-835a-937bf473339a', url: 'https://github.com/suma-ec-projects-july/maven-web-application.git' 
}
stage('Build')
{
    sh "${mavenhome}/bin/mvn clean package"
}
stage('sonarqube report')
{
    sh "${mavenhome}/bin/mvn clean sonar:sonar"
}
stage('upload artifacts into nexus')
{
    sh "${mavenhome}/bin/mvn clean deploy"
}

stage('Deploy application into Tomcat Server')
{
sshagent(['ed266360-3db9-4609-bc35-86d6c0ea801d']){
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.4.59:/opt/apache-tomcat-9.0.52/webapps/"
}
}
stage('send Email notification')
{
emailtext body: '''Finished the build...!


Regards,
Sumalatha,
''', subject: 'Finished the build..!', to: 'dreamatdevops@gmail.com'
    
}
}
