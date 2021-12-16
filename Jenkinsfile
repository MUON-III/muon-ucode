
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
        sh 'mkdir build'
        sh 'cd build && $WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode.rom --ucode'
        sh 'cd build && $WORKSPACE/casm-static --input $WORKSPACE/ucode.txt --output ucode.bin --ucode --binary'
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
