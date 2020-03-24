pipeline {
    // run on jenkins nodes tha has java 8 label
    agent any
    // global env variables
    environment {
        EMAIL_RECIPIENTS = 'mahmoud.romeh@test.com'
    }
    stages {
        stage('Release and publish artifact') {
            steps {
                script {
                    def mvnHome = tool 'Maven 3.3.9' //
                    if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
                        def v = getReleaseVersion()
                        releasedVersion = v;
                        if (v) {
                            echo "Building version ${v} - so released version is ${releasedVersion}"
                        }
                        sh "'${mvnHome}/bin/mvn' -Dmaven.test.skip=true  versions:set  -DgenerateBackupPoms=false -DnewVersion=${v}"
                        sh "'${mvnHome}/bin/mvn' -Dmaven.test.skip=true clean deploy"

                    } else {
                        error "Release is not possible. as build is not successful"
                    }
                }
            }
        }
    }
}

def getReleaseVersion() {
            def pom = readMavenPom file: 'pom.xml'
            def versionNumber;
            if (gitCommit == null) {
                versionNumber = "1.0.1";
            } else {
                versionNumber = "1.0.0";
            }
            echo  pom.version.replace("-SNAPSHOT", ".${versionNumber}")
            return pom.version.replace("-SNAPSHOT", ".${versionNumber}")
        }

