
pipeline {
  agent {
    label '!windows-1' 
  }
  environment {
    CASM_URL = 'https://jenkins.i-am.cool/job/muon-casm/job/master/lastSuccessfulBuild/artifact/casm-staticlatest' 
    DISCORD_URL = credentials("muon-discord-webhook")
  }
  stages {
    stage('Assemble microcode') {
      steps {
        sh 'curl -L "$CASM_URL" -o casm-static && chmod +x casm-static'
        dir("build"){
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode-lower.rom --ucodesplit /dev/null --ucode'
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode.bin --ucode --binary'
          archiveArtifacts artifacts: 'ucode*', fingerprint: true
          deleteDir()
        }
      }
    }
  }
  
  post {
      cleanup { 
          discordSend description: "Microcode build", footer: "Build done", link: "$BUILD_URL", result: currentBuild.currentResult, title: JOB_NAME, webhookURL: DISCORD_URL
          cleanWs() 
      }
  }
}
