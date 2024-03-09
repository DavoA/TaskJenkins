pipeline {
    agent any
    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'targetBranchName', value: '$.pull_request.base.ref'],
                [key: 'sourseBranchName', value: '$.pull_request.head.ref'],
            ],
            token: 'my-webhook-token'
        )
    }
    stages {
        stage('Check branch name') {
            steps {
                script {
                    if (env.tagetBranchName == 'dev') {
                        echo "Branch name is 'dev', proceeding with the merge"
                    } else {
                        error "Merge rejected!"
                    }
                }
            }
        }
        stage('Merge to main') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'davoa-github-pat', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        git branch: 'dev', url: 'https://github.com/DavoA/TaskJenkins.git'
                        git commit: 'Merging dev into main', url: 'https://github.com/DavoA/TaskJenkins.git'
                        git mergeRemote: 'origin', remote: env.sourseBranchName
                    }
                }
            }
        }
    }
}
