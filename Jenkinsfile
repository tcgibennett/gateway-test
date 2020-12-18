node {
    stage('Example') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
            container('git') {
                        echo "Search for changes in '${gitBranch}'"

                        changed_directories = sh(
                                script: """
                    git remote add github https://github.com/tcgibennett/gateway-test.git;
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
        } else {
            echo 'I execute elsewhere'
        }
    }
}

