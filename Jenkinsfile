pipeline {
    // run on jenkins nodes tha has java 8 label
    agent any
    // global env variables
    environment {
        RELVER = "2.2.2"
    }
    stages {
        stage('Release and publish artifact') {
          environment {
                 RELVER = "${RELVER}"
          }
            steps {
                script {
            def pom = readMavenPom file: 'pom.xml'
            def version = pom.version.replace("-SNAPSHOT", ".${RELVER}")
            stage 'Build'
            sh "/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven_3.3.9/bin/mvn -DreleaseVersion=${version} -DpushChanges=false -DlocalCheckout=true -DpreparationGoals=initialize release:prepare release:perform -B"
                } 
            }
        }
    }
}
