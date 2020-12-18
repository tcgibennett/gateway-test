node {
    stage('Example') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
            git diff-tree --no-commit-id --name-only -r ${env.GIT_COMMIT}
        } else {
            echo 'I execute elsewhere'
        }
    }
}

