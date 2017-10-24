properties([
    buildDiscarder(logRotator(numToKeepStr: '5')),
    pipelineTriggers([
        pollSCM('H/5 * * * *')
    ])
])

timestamps() {
    timeout(time: 10, unit: 'MINUTES') {
        node(env.JOB_NAME.split('/')[0]) {
            stage('Greeting') {
                withEnv(["JAVA_HOME=${tool 'jdk8_oracle'}", "PATH+MAVEN=${tool 'maven35'}/bin:${env.JAVA_HOME}/bin"]) {
                    echo 'Scripted Hello, World!'

                    sh "java --version"

                    sh "mvn --version"
                }
            }
        }
    }
}
