properties([
    buildDiscarder(logRotator(numToKeepStr: '5')),
    pipelineTriggers([
        pollSCM('H/5 * * * *')
    ])
])

timestamps() {
    timeout(time: 10, unit: 'MINUTES') {
        node {
            stage('Greeting') {
                withEnv(['GREETINGS_TO=Jenkins Techlab']) {
                    echo "Scripted Hello, ${env.GREETINGS_TO} !"

                    // also available as env variable to a process:
                    sh 'echo "Scripted Hello, $GREETINGS_TO !"'
                }
            }
        }
    }
}
