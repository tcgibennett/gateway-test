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

        def changedCharts = null
            changedCharts = sh(
                                        script: """
                            CHANGED_FOLDERS=`git diff --find-renames --name-only \$(${getChangedFolderGitCommand(gitBranch)}) ./ | awk -F/ '{print \$1"/"\$2}' | uniq`;
                            if [ -z \"\$CHANGED_FOLDERS\" ]; then
                            printf \"No changes to charts found\";
                            exit 0;
                            fi;
                            for directory in \${CHANGED_FOLDERS}; do
                            if [ -d \${directory} ]; then
                                echo \${directory};
                            fi
                            done;
                            """,
                                        returnStdout: true
                                )
    }
}