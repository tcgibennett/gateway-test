def test() {
    node {
        stage('Example') {
            if (env.BRANCH_NAME == 'master') {
                echo 'I only execute on the master branch'
            } else {
                echo 'I execute elsewhere'
            }
        }
    }
}

try {
    test
} catch(err) {
    echo "Error handled: ${err}"
}