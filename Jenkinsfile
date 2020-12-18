node {
    stage('Example') {
        echo 'Example1 Stage'
        echo env.BRANCH_NAME
        def changedCharts = null
        changedCharts = sh(
                                    script: """
                        git remote add github https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/tcgibennett/gateway-test.git;
                        git fetch github master;
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