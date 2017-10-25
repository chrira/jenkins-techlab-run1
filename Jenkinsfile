properties([
    buildDiscarder(logRotator(numToKeepStr: '5')),
    pipelineTriggers([
        pollSCM('H/5 * * * *')
    ])
])

try {
    timestamps() {
        timeout(time: 10, unit: 'MINUTES') {
            node(env.JOB_NAME.split('/')[0]) {
                stage('Build') {
                    try {
                        withEnv(["JAVA_HOME=${tool 'jdk8_oracle'}", "PATH+MAVEN=${tool 'maven35'}/bin:${env.JAVA_HOME}/bin"]) {
                            checkout scm
                            sh 'mvn -B -V -U -e clean verify -Dsurefire.useFile=false -Dmaven.test.failure.ignore=true'
                            archiveArtifacts 'target/*.?ar'
                        }
                    } finally {
                        junit 'target/**/*.xml'  // Requires JUnit plugin
                    }
                }
            }

        }
    }
} catch (e) {
    node {
        mail to: "raaflaub@puzzle.ch", subject: "Build failure - ${env.JOB_NAME} ${env.BUILD_NUMBER}",
             body: "Build failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}\nSee results in Jenkins: <${env.BUILD_URL}>\n\n${e.stackTrace}"
    }
    throw e
} finally {
    node {
        if (currentBuild.result == 'UNSTABLE') {
            mail to: "raaflaub@puzzle.ch", subject: "Build unstable - ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                 body: "Unstable build: ${env.JOB_NAME} ${env.BUILD_NUMBER}\nSee results in Jenkins: <${env.BUILD_URL}>"
        } else if (currentBuild.result == null) { // null means success
            mail to: "raaflaub@puzzle.ch", subject: "Build success - ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                 body: "Success of build: ${env.JOB_NAME} ${env.BUILD_NUMBER}\nSee results in Jenkins: <${env.BUILD_URL}>"
        }
    }
}
