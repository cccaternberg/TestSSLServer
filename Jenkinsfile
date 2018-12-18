/**
  Workfow to download and execute a SSL Test connexion
  @author kuisatahverat
 **/
env.targetHost = targetHost  
def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label, containers: [
    containerTemplate(name: 'maven', image: 'maven:3.3.9-jdk-8-alpine', ttyEnabled: true, command: 'cat')
  ]) {
  node (label) {
    stage ('Env'){
      sh 'export'
    }
    stage ('Download sources'){
      tool name: 'Default', type: 'hudson.plugins.git.GitTool'
      git 'https://github.com/kuisathaverat/TestSSLServer.git'
    }
    container('maven') {
        stage ('Compile sources'){      
         // tool name: 'maven3', type: 'hudson.tasks.Maven$MavenInstallation'
          sh 'mvn clean compile'
        }
        stage ('Execute Test SSL support features'){
          sh 'mvn -Djavax.net.debug=ssl:handshake -Dexec.mainClass=org.bolet.TestSSLServer -Dexec.args=${targetHost} exec:java'
        }
        stage ('Execute Test SSL handshake'){
          sh 'mvn -Djavax.net.debug=ssl:handshake -Dexec.mainClass=org.bolet.TestSSLHandShake -Dexec.args=https://${targetHost} exec:java'
       }
    }
  }
}
