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
                tv = sh(
                    script: """
                     printf \"Hello Test\";
                    """,
                    returnStdout: true
                )
            }
        }
    }
}