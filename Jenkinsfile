def getChangedFolderGitCommand(branchName) {
    if (branchName == 'origin/master') {
        'git rev-parse HEAD'
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
                            PWD=`pwd`;
                            export GOPATH=/home/tbennett/go;
                            export PATH=$GOPATH/bin:$PATH
                            CHANGED=`git diff-tree --no-commit-id --name-only -r \$(${getChangedFolderGitCommand(gitBranch)})`;
                            if [ -z \"\$CHANGED\" ]; then
                            printf \"No changes to charts found\";
                            exit 0;
                            fi;
                            for oas in \${CHANGED}; do
                            if [[ (-f \${oas} && \${oas} == *"OAS30.json"*)]]; then
                                echo \${PWD}/\${oas};
                            fi
                            done;
                            apigtw-ctrl --help;
                            """,
                                        returnStdout: true
                                )
    }
}