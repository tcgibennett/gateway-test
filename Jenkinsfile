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
                def changedCharts = null
                changedCharts = sh(
                    script: """
                     printf \"Hello Test\";
                    """,
                    returnStdout: true
                )
            }
        }
    }
}