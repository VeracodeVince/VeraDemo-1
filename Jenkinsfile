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
                          --veracode_profile "credentials" \
                          --file "**/**.war" \
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
