
pipeline {
  agent {
    label '!windows-1' 
  }
  environment {
    CASM_URL = 'https://jenkins.i-am.cool/job/muon-casm/job/master/42/artifact/casm-static-48b96d4-git' 
  }
  stages {
    stage('Assemble microcode') {
      steps {
        sh 'curl -L "$CASM_URL" -o casm-static && chmod +x casm-static'
        dir("build"){
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode-lower.rom --ucodesplit ucode-upper.rom --ucode'
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode.bin --ucode --binary'
          archiveArtifacts artifacts: 'ucode*', fingerprint: true
          deleteDir()
        }
      }
    }
  }
  
  post {
      cleanup { 
          cleanWs() 
      }
  }
}
