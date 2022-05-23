node(){
 stage("git init"){
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rachamallasandhya/mavenproj.git']]])
}
 stage("maven build"){
   sh '''mvn package'''
   
 }
 stage("sonarqube"){
   sh '''mvn sonar:sonar \\
  -Dsonar.projectKey=mavenpipeline \\
  -Dsonar.host.url=http://18.220.31.152:9000 \\
  -Dsonar.login=033f77a0e783bff64b4327aa7fb8941e1394468b'''
 }
 stage("nexus"){
   nexusArtifactUploader artifacts: [[artifactId: '$BUILD_TIMESTAMP', classifier: '', file: 'webapp/target/webapp.war', type: 'war']], credentialsId: 'NEXUS_CRED', groupId: 'DEV', nexusUrl: 'http://3.17.138.160:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'nexus-release', version: '$BUILD_ID'
   
 }
 stage("tomcat"){
   deploy adapters: [tomcat9(credentialsId: 'TOMCAT_CREDS1', path: '', url: 'http://3.133.144.30:8087/')], contextPath: 'devenvtest', war: '**/*.war'
 }
}
