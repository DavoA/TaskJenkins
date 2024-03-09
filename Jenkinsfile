pipeline {
    agent any
    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'branchName', value: '$.body.head.ref'],
            ],
            token: 'my-webhook-token'
        )
    }
    stages {
        stage('Check branch name') {
            steps {
                script {
                    if (env.branchName != 'dev') {
                        echo "Branch name is not 'dev', proceeding with the merge"
                    } else {
                        error "Merge rejected! Branch name 'dev' is not allowed."
                    }
                }
            }
        }
        stage('Merge to main') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'davoa-github-pat', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        git branch: 'main', url: 'https://github.com/DavoA/TaskJenkins.git'
                        git commit: 'Merging dev into main', url: 'https://github.com/DavoA/TaskJenkins.git'
                        git mergeRemote: 'origin', remote: 'dev'
                    }
                }
            }
        }
    }
}
//https://smee.io/MVGyqPCwarxBXwu
