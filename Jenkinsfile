pipeline {
    agent { label env.JOB_NAME.split('/')[0] }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Requires the "Timestamper Plugin"
    }
    triggers {
        pollSCM('H/5 * * * *')
    }
    tools {
        jdk "jdk8"
        'com.cloudbees.jenkins.plugins.customtools.CustomTool' "maven35"
    }
    stages {
        stage('Build') {
            steps {
                sh 'java -version'

                sh 'mvn --version'
            }
        }
    }
}
