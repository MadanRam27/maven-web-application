node
{

def mvnHome = tool name: 'maven3.9.1'

stage('GetCodeFromGit'){
  git branch: 'development', credentialsId: 'd528efbd-5aa4-493c-b7be-b6fd23832ac3',
  url: 'https://github.com/MadanRam27/maven-web-application.git' 
}

stage('Build'){
    sh "${mvnHome}/bin/mvn clean package"
}

stage('SonarQubeReport'){
     sh "${mvnHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsNexus'){
     sh "${mvnHome}/bin/mvn deploy"
}

stage('DeployApptToTomcatserver'){
    sshagent(['cc34a2b5-3d3e-413c-ad68-66c969850812']) {
   sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.73.14:/opt/apache-tomcat-9.0.74/webapps/"
}
}

stage('SendEmail'){
    emailext body: 'Buld is succesful', subject: 'Buld success', to: 'madanram2726@gmail.com'
}


}
