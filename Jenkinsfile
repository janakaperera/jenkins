pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout poll: false, scm: [$class: 'SubversionSCM', additionalCredentials: [], excludedCommitMessages: '', excludedRegions: '', excludedRevprop: '', excludedUsers: '', filterChangelog: false, ignoreDirPropChanges: false, includedRegions: '', locations: [[credentialsId: '1', depthOption: 'infinity', ignoreExternalsOption: false, local: 'codebase', remote: 'https://repos.orangehrm.com/enterprise/tags/6.2/symfony/plugins/orangehrmAttendancePlugin/lib/dao/']], quietOperation: false, workspaceUpdater: [$class: 'UpdateWithCleanUpdater']]
                sh 'git clone https://github.com/janakaperera/semgrep_rules'
                sh 'cd codebase'
           }
        }
        stage('Script') {
            steps {
                sh '''
                    #!/bin/bash
                    cd /var/lib/jenkins/workspace/semgrep_scm
                    semgrep --config /var/lib/jenkins/workspace/semgrep_scm/semgrep_rules > /home/administrator/Desktop/bash_scripts/result_$(date +%F.%T).txt

                '''
            }
        }
        stage('archive') {
            steps {
                archiveArtifacts(artifacts: 'result_$(date +%F.%T).txt', followSymlinks: false)
           }
        }
        stage('Clean') {
            steps {
                cleanWs()
           }
        }
    }
}
