pipeline {
    agent any
    parameters{
        text(name: 'EMAIL_LIST', defaultValue: 'dheegayu@orangehrmlive.com', description: 'Email List')
        text(name: 'CODEBASE_PATH', defaultValue: 'https://repos.orangehrm.com/enterprise/trunk/', description: 'Codebase Path')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout poll: false, scm: [$class: 'SubversionSCM', additionalCredentials: [], excludedCommitMessages: '', excludedRegions: '', excludedRevprop: '', excludedUsers: '', filterChangelog: false, ignoreDirPropChanges: false, includedRegions: '', locations: [[credentialsId: '1', depthOption: 'infinity', ignoreExternalsOption: false, local: 'codebase', remote: '$CODEBASE_PATH']], quietOperation: false, workspaceUpdater: [$class: 'UpdateWithCleanUpdater']]
                sh 'git clone https://github.com/janakaperera/semgrep_rules'
           }
        }
        stage('Script') {
            steps {
                sh '''
                    #!/bin/bash
                    cd /var/lib/jenkins/workspace/semgrep_scm/codebase
                    semgrep --config /var/lib/jenkins/workspace/semgrep_scm/semgrep_rules > /var/lib/jenkins/workspace/semgrep_scm/results/result_$(date +%F.%T).txt
                '''
            }
            post {
            always {
                script{
                    echo $java.time.LocalDate.now()
                    echo "Complated the script stage"
                    sh "cd /var/lib/jenkins/workspace/semgrep_scm/results"
                    lastFile = sh (
                    script: 'ls /var/lib/jenkins/workspace/semgrep_scm/results -Ar | head -1',
                    returnStdout: true
                    ).trim()
            }
               archiveArtifacts "results/$lastFile" 
               echo '${FILE,path="results/$lastFile"}'
           }
         } 
            
               
        }
         
    }
}

