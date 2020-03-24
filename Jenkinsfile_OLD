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
                    if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
                        def v = getReleaseVersion("${RELVER}")
                        releasedVersion = v;
                        if (v) {
                            echo "Building version ${v} - so released version is ${releasedVersion}"
                        }
                        sh "'${mvnHome}/bin/mvn' -B release:prepare"
                        sh "'${mvnHome}/bin/mvn' -B release:perform"

                    } else {
                        error "Release is not possible. as build is not successful"
                    }
                }
            }
        }
    }
}

def getReleaseVersion(RELVER) {
            def pom = readMavenPom file: 'pom.xml'
    def versionNumber  = "${RELVER}";
            
            echo  pom.version.replace("-SNAPSHOT", ".${versionNumber}")
            return pom.version.replace("-SNAPSHOT", ".${versionNumber}")
        }

