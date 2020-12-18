def testme {
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
}

try {
// trigger parallel execution
    testme
} catch (err) {
    echo "Error handled: ${err}"
}