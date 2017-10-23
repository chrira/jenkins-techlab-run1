pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Requires the "Timestamper Plugin"
    }
    triggers {
        pollSCM('H/5 * * * *')
    }
    environment {
        GREETINGS_TO = 'Jenkins Techlab'
    }
    stages {
        stage('Greeting') {
            steps {
                echo "Declarative Hello, ${env.GREETINGS_TO} !"

                // also available as env variable to a process:
                sh 'echo "Declarative Hello, $GREETINGS_TO !"'
            }
        }
    }
}
