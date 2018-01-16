pipeline {
  agent any

  stages{
    stage('build'){
      steps{
      sh 'ant -f build.xml -v'
        }
    }
    stage('deploy"){
      steps{
        sh "cp /dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/jar"
       }
  }
}
  post{
    always{
      archiveArtifacts artifacts: 'dist/*jar', fingerprint: true
          }
      }
}
