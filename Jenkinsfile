pipeline {
    // run on jenkins nodes tha has java 8 label
    agent any
    // global env variables
    environment {
        RELVER = "0.0.2"
    }
    stages {
        stage('Release and publish artifact') {
          environment {
                 RELVER = "${RELVER}"
          }
            steps {
                script {
                    def mvnHome = tool 'Maven 3.3.9' //
                    getReleaseVersion("${RELVER}")
                } 
            }
        }
    }
}

def getReleaseVersion(RELVER) {
            def pom = readMavenPom file: 'pom.xml'
    def versionNumber  = "${RELVER}";
         pom.version.replace("-SNAPSHOT", ".${versionNumber}")
         sh "'${mvnHome}/bin/mvn' -B release:prepare"
         sh "'${mvnHome}/bin/mvn' -B release:perform"
        }

