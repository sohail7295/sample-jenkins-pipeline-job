pipeline {
  agent any
  tools {
        maven 'M3'
    }
  stages {
    stage ('checkout code') {
      steps {
        git 'https://github.com/sohail7295/crudApp'
      }
   }
    stage ('Run Build') {
      steps {
        sh "/opt/apache-maven-3.3.9/bin/mvn clean package"
      }
    }
    
     stage ('Nexus') {
      steps {
      nexusArtifactUploader artifacts: [[artifactId: 'crudApp', classifier: '', file: 'target/crudApp.war', type: 'war']], credentialsId: 'Nexus', groupId: 'Central', nexusUrl: '3.17.203.182:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'releases', version: '3.4'
      }
    }

    
    stage ('Copy to tomcat') {
      steps {
        sh "sudo wget http://3.17.203.182:8081/nexus/service/local/repositories/releases/content/Central/crudApp/3.4/crudApp-3.4.war -O /opt/apache-tomcat-9.0.14/webapps/crudApp.war"
        sh "sudo sh /opt/apache-tomcat-9.0.14/bin/catalina.sh stop"
        sh "sudo sh /opt/apache-tomcat-9.0.14/bin/catalina.sh start"
      }
    }
  }
}
