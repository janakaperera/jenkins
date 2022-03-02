pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout poll: false, scm: [$class: 'SubversionSCM', additionalCredentials: [], excludedCommitMessages: '', excludedRegions: '', excludedRevprop: '', excludedUsers: '', filterChangelog: false, ignoreDirPropChanges: false, includedRegions: '', locations: [[credentialsId: '1', depthOption: 'infinity', ignoreExternalsOption: false, local: 'codebase', remote: 'https://repos.orangehrm.com/enterprise/tags/6.2/']], quietOperation: false, workspaceUpdater: [$class: 'UpdateWithCleanUpdater']]
                    
           }
        }
        stage('Script') {
            steps {
                sh '''
                    #!/bin/bash
                    cd /var/lib/jenkins/workspace/semgrep_scm
                    semgrep --config /home/administrator/Downloads/orangehrm123/orangehrm123/rules/enterprise.yaml > /var/lib/jenkins/workspace/semgrep_scm/result_$(date +%F.%T).txt

                '''
            }
        }
        stage('Clean') {
            steps {
                cleanWs()
           }
        }
    }
}
