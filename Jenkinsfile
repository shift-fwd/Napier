pipeline {
    agent any
    environment {
        GRADLE_CMD = './gradlew --console=plain --warning-mode=none '
        JENKINS_NODE_COOKIE = 'dontKillMe'
        NEXUS_CREDS = credentials('shift-nexus')
    }

    stages {
        stage('Build and Test') {
            steps {
                sh "${env.GRADLE_CMD} clean build"
                script {
                    env.NEW_VERSION = "${sh(returnStdout: true, script: "${env.GRADLE_CMD}  -P release")}".trim()
                }
            }
        }

        stage('Push to Nexus and Tag') {
            when {
                branch 'master'
            }
            steps {
                sh "${env.GRADLE_CMD} publish -Prelease"
            }
        }
    }
}
