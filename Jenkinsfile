pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Hello Build'
            }
        }
        stage('Test') {
            steps {
                sh(
                    script: """
                     printf \"Hello Test\";
                    """,
                    returnStdout: true
                )
            }
        }
    }
}