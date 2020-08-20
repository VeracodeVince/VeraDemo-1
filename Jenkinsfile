pipeline {
  agent any
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
                        jar xf pipeline-scan-LATEST.zip pipeline-scan.jar
                        java -jar pipeline-scan.jar \
                          --veracode_api_id "57bbc77634c8f00dd89ea4fe0b9ef9c6" \
                          --veracode_api_key "4eb31832346d1a8455b676f6dd76ef5f924449a59cf9cdae22a5cb3ef5599ffc6d24b24339eb65d992c05dab6cb4d133660ec60ff82985c6303df6f6727aa7dd" \
                          --file "build/libs/sample.jar" \
                          --fail_on_severity="Very High, High" \
                          --fail_on_cwe="80" \
                          --baseline_file "${CI_BASELINE_PATH}" \
                          --timeout "${CI_TIMEOUT}" \
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
