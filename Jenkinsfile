
pipeline {
  agent any
  stages {
    stage('Build CASM') {
      steps {

      }
    }
    stage('Assemble') {
      steps {
        sh 'mkdir build'
        sh 'cd build && cmake .. -G "Unix Makefiles"'
        sh 'cd build && make'
      }
    }
  }
  
  post {
      always {
          archiveArtifacts artifacts: 'build/casm.exe', fingerprint: true
      }
  }
}
