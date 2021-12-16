
pipeline {
  agent {
    label '!windows-1' 
  }
  environment {
    CASM_VERSION = 'v1.1.1' 
  }
  stages {
    stage('Build CASM') {
      steps {
        sh 'git clone --branch $CASM_VERSION https://github.com/MUON-III/muon-casm.git'
        sh 'cd muon-casm && mkdir build'
        sh 'cd muon-casm/build && cmake ..'
        sh 'cd muon-casm/build && make'
        stash includes: 'muon-casm/build/casm', name: 'builtCASM'
      }
    }
    stage('Assemble') {
      steps {
        unstash 'builtCASM'
        sh 'mkdir build'
        sh 'cd build && $WORKSPACE/muon-casm/build/casm --input $WORKSPACE/ucode.txt --output ucode.rom --ucode'
        sh 'cd build && $WORKSPACE/muon-casm/build/casm --input $WORKSPACE/ucode.txt --output ucode.bin --ucode --binary'
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
