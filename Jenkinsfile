def getChangedFolderGitCommand(branchName) {
    if (branchName == 'origin/master') {
        'git rev-parse HEAD~1'
    } else {
        'git merge-base github/master HEAD'
    }
}

node {
    def repo = checkout scm
    def gitBranch = repo.GIT_BRANCH
    def gitCommit = repo.GIT_COMMIT
    def shortGitCommit = "${gitCommit[0..10]}"

    
    stage('Example') {
        echo 'Example1 Stage'
        echo gitBranch

        def oasFiles = null
            oasFiles = sh(
                                        script: """
                            CHANGED=`git diff --find-renames --name-only \$(${getChangedFolderGitCommand(gitBranch)}) ./ | awk -F/ '{print \$1"/"\$2}' | uniq`;
                            if [ -z \"\$CHANGED\" ]; then
                            printf \"No changes to charts found\";
                            exit 0;
                            fi;
                            for oas in \${CHANGED}; do
                            if [ (-f \${oas} && grep -q 'OAS30.json' \${oas})]; then
                                echo \${oas};
                            fi
                            done;
                            """,
                                        returnStdout: true
                                )
    }
}