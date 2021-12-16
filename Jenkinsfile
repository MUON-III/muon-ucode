
pipeline {
  agent {
    label '!windows-1' 
  }
  environment {
    CASM_URL = 'https://jenkins.i-am.cool/job/muon-casm/job/master/41/artifact/casm-static-624c119-git' 
  }
  stages {
    stage('Assemble microcode') {
      steps {
        sh 'curl -L "$CASM_URL" -o casm-static && chmod +x casm-static'
        dir("build"){
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode.rom --ucode'
          sh '$WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode.bin --ucode --binary'
          deleteDir()
        }
      }
    }
  }
  
  post {
      always {
          archiveArtifacts artifacts: 'build/ucode*', fingerprint: true
      }
      cleanup { 
          cleanWs() 
      }
  }
}
