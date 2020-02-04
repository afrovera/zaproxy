pipeline {
    agent any
    stages {
        stage('Spin-up OWASP ZAP Container') { 
            steps {
		sh "docker rm owasp_zapcontainer"
		sh "docker run --name owasp_zapcontainer -u root -v $PWD:/zap/wrk -t owasp/zap2docker-weekly zap-baseline.py -t http://172.18.0.9:8080/ -g gen.conf -a -j -r owasp_zap_report.html || true"
		sh "mkdir -p $JENKINS_HOME/jobs/$JOB_NAME/builds/$BUILD_NUMBER/htmlreports/OWASP_20ZAP_20REPORT"
		sh "docker cp owasp_zapcontainer:/zap/wrk/owasp_zap_report.html $JENKINS_HOME/jobs/$JOB_NAME/builds/$BUILD_NUMBER/htmlreports/OWASP_20ZAP_20REPORT"
  	    }
        }
    }
    post {
        always {
	    publishHTML target: [
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: true,
            reportDir: '$JENKINS_HOME/jobs/$JOB_NAME/builds/$BUILD_NUMBER',
            reportFiles: 'owasp_zap_report.html',
            reportName: 'OWASP ZAP REPORT'
          ]
        }
    }

}
