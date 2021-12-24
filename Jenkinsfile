
pipeline {
  agent {
    label '!windows-1' 
  }
  environment {
    CASM_URL = 'https://jenkins.i-am.cool/job/muon-casm/job/master/lastSuccessfulBuild/artifact/casm-staticlatest' 
    DISCORD_URL = credentials("muon-discord-webhook")
    GCP_IAM_KEY = 'gcp-iam-key'
  }
  stages {
    stage('Assemble microcode') {
      steps {
        sh 'curl -L "$CASM_URL" -o casm-static && chmod +x casm-static'
        dir("build"){
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode-lower.rom --ucodesplit /dev/null --ucode'
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode.bin --ucode --binary'
          archiveArtifacts artifacts: 'ucode*', fingerprint: true
          step([$class: 'ClassicUploadStep', credentialsId: env
                        .GCP_IAM_KEY,  bucket: "gs://muon-3",
                      pattern: "ucode.bin"])
          deleteDir()
        }
      }
    }
  }
  
  post {
    cleanup { 
        cleanWs() 
    }
    success {
     discordSend description: "Build success", footer: "", link: "$BUILD_URL", result: currentBuild.currentResult, title: JOB_NAME, webhookURL: DISCORD_URL  
    }
    failure {
     discordSend description: "Build failed", footer: "", link: "$BUILD_URL", result: currentBuild.currentResult, title: JOB_NAME, webhookURL: DISCORD_URL
    }
  }
}
