def chartPipelineTasks = [:]

properties([buildDiscarder(logRotator(numToKeepStr: '20', artifactNumToKeepStr: '5')),
            parameters([
                    text(name: 'HELM_CHARTS_TO_BUILD', defaultValue: '', description: 'List of helm charts to build separated with a line feed')
            ]),
            disableConcurrentBuilds()
])

podTemplate(label: label, containers: [
        containerTemplate(name: 'git', image: 'alpine/git', command: 'cat', ttyEnabled: true)
],

        serviceAccount: 'helm-charts-service-account') {

    node(label) {
        def repo = checkout scm
        def gitBranch = repo.GIT_BRANCH
        def gitCommit = repo.GIT_COMMIT
        def shortGitCommit = "${gitCommit[0..10]}"

        def artifactory = Artifactory.server 'talendregistry-onprem'
        artifactory.credentialsId = 'artifactory-datapwn-credentials'

        notifyMessengers()

        // array of changed charts (folder), empty by default
        def changedCharts = null

        if (env.BRANCH_NAME == 'master') {
            stage('Search for charts to build') {
                withCredentials([usernamePassword(credentialsId: 'tcgibennett', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    container('git') {
                                echo "Search for changes in '${gitBranch}'"

                                changed_directories = sh(
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
            }
        } 
    }
}

try {
    parallel testme
} catch(err) {
    echo "Error handled: ${err}"
}