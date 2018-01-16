pipeline {
  agent any

  stages{
    stage('build'){
      steps{
      sh 'ant -f build.xml -v'
        }
    }
    stage('deploy'){
      steps{
        sh "cp dist/rectangle_${BUILD_NUMBER}.jar /var/www/html/rectangle/jar/"
       }
  }
}
  post{
    always{
      archiveArtifacts artifacts: 'dist/*jar', fingerprint: true
          }
      }
}
