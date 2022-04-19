node{
def mavenHome = tool name: "maven3.8.5"

//Get the code from GitHub repo
stage('CheckoutCode'){
git branch: 'development', credentialsId: 'd1ddc1da-8081-4cea-b354-bb93ad41877b', url: 'https://github.com/rahim-ameen/maven-web-application.git' 
}

//Build by Maven
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

//Generate SonarQube
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//Nexus
stage('UploadArtifactIntoNexusRepo'){
sh "${mavenHome}/bin/mvn deploy"
} 

stage('DeployApplicationIntoTomcat')
{
sshagent(['f4e84f39-1a4e-4cb0-8198-d1b7eabe53d1']){
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@3.111.53.227:/opt/apache-tomcat-9.0.60/webapps/"  
} 
}

} //NodeClose
