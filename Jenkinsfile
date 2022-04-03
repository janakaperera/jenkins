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
                    sh "cd /var/lib/jenkins/workspace/semgrep_scm/results"
                    lastFile = sh (
                    script: 'ls /var/lib/jenkins/workspace/semgrep_scm/results -Ar | head -1',
                    returnStdout: true
                    ).trim()
                    sh (
		    script: '''awk 'BEGIN{
	print "<html> <body> <TABLE border=0 cellspacing=3 cellpadding=3 width=100% padding = 10px>"
    print "<tr bgcolor=green height = 50px>" "<td>" "<font size=6 color=white>" "<b>" "<center>" "Semgrep Results" "</center>" "</b>" "</font>" "</td>" "</tr>"
    print "<tr bgcolor=blue height = 30px>" "<td>" "<font size=4.4 color=white>" "<b>" "<center>" "Date" "</center>" "</b>" "</font>" "</td>" "</tr>"
    print "<tr height = 30px>" "<td>" "<font size=4.4 color=black>"  "<b>" "<center>" "Executed date is: "strftime("%F") "</center>" "</b>" "</font>" "</td>" "</tr>"
    print "<tr bgcolor=blue height = 30px>" "<td>" "<font size=4.4 color=white>" "<b>" "<center>" "Findings" "</center>" "</b>" "</font>" "</td>" "</tr>"
}

{  
	print "<TR>"
        
		if (substr($1,1,1) ~ /^[0-9]/ )
			print "<tr>" "<td>""<font size =2> "  $i "</font>" "</td>""</tr>"
		if (/rule/)
                        print "<tr height = 20px>" "<td>" "<font size = 2>" $i "</font>" "</td>" "</tr>"
		if (/symfony/)
                        print "<tr bgcolor=red height = 30px>" "<td>" "<font color=white size=3>" "<b>" $i "</b>" "</font>" "</td>" "</tr>"
		if (/upgrader/)
                        print "<tr bgcolor=red height = 30px>" "<td>" "<font color=white size=3>" "<b>" $i "</b>" "</font>" "</td>" "</tr>"
	
	print "</TR>"
}
END{
	print "</TABLE> </body> </html>"
}'$lastFile > results_html/result_$(date +%F.%T).html''')
                    lastHTMLFile = sh (
                    script: 'ls /var/lib/jenkins/workspace/semgrep_scm/results_html -Ar | head -1',
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

