pipeline {
    agent any
    stages {
        stage('Build stage') {
            steps {
              sh "docker run --name zapcontainer -u root -v $PWD:/zap/wrk -t owasp/zap2docker-weekly zap-baseline.py -t http://0.0.0.0:8080 -g gen.conf - a -j -r owaspzap_report.html || true"
              sh "mkdir -p $JENKINS_HOME/jobs/$JOB_NAME/builds/$BUILD_NUMBER/htmlreports/OWASP_ZAP"
              sh "docker cp zapcontainer:/zap/wrk/owaspzap_report.html $JENKINS_HOME/jobs/$JOB_NAME/builds/$BUILD_NUMBER/htmlreports/OWASP_ZAP"
} }
} post {
        always {
           publishHTML target: [
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: true,
            reportDir: '$JENKINS_HOME/jobs/$JOB_NAME/builds/$BUILD_NUMBER',
            reportFiles: 'owaspzap_report.html',
            reportName: 'OWASP ZAP REPORT'
          ]
} }
}