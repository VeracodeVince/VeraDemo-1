pipeline {
  agent any
  environment {
        vid = credentials('vid')
        vkey = credentials('vkey')
    }
  stages {
    stage('Maven Build') {
      steps {
            bat """\
              mvn clean verify
                """
      }
    }
    stage('Veracode Pipeline Scan') {
      steps {
              bat """\
                        curl -O https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip
                        7z.exe x pipeline-scan-LATEST.zip pipeline-scan.jar -y
                        java -jar pipeline-scan.jar \
                          --veracode_api_id "${env.vid}" \
                          --veracode_api_key "${env.vkey}" \
                          --file "target/verademo.war" \
                          --fail_on_severity="Very High, High" \
                          --project_name "${env.JOB_NAME}" \
                          --project_url "${env.GIT_URL}" \
                          --project_ref "${env.GIT_COMMIT}" \
                   """
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: 'results.json', fingerprint: true
    }
  }
}
