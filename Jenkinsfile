pipeline {
  agent none

  stages{
    stage('build'){
      agent{
       label 'app'
      }
      steps{
      sh 'ant -f build.xml -v'
        }
      post{
        success{
          archiveArtifacts artifacts: 'dist/*jar', fingerprint: true
          }
      }
    }
   }
  }
