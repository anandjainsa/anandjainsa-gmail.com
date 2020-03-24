@Library('mylibrary')_
pipeline {

    agent any

    tools {

        // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)

        //maven "maven"
        maven "Maven 3.3.9"
        jdk "jdk1.8.0_131"
    }

    environment {
        //  Define all variables

        SERVICEPORT = "8081"
        APPNAME = "memberproxyapi"
        NAMESPACE = "ns-middleware-dv1"
        ENVIRONMENT = "dv1"
        ING_SECRET = "w-ttgtpmg-net-secret"
        ING_HOST = "npworker.ttgtpmg.net"
        RELVER = "2.2.2"
    }

    stages {
        stage('Release and publish artifact') {
            steps {
                mavenRelease("${RELVER}")
            }
        }
        stage('Building and Pushing Container Image') {
            steps {
                dockerBuild()
            }
        }

        stage("Deploying Application to Prod") {
            environment {
                NAMESPACE = "${NAMESPACE}"
                ENVIRONMENT = "${ENVIRONMENT}"
                SECRET = "${ING_SECRET}"
                HOST = "${ING_HOST}"
            }
            steps {
                envCreate("${ENVIRONMENT}")
                kubeDeployment("${NAMESPACE}", "${APPNAME}", "${SERVICEPORT}", "${ENVIRONMENT}", "${SECRET}", "${HOST}")
            }
        }
    }
}
