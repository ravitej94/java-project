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
    stage('deploy'){
      agent{
       label 'app'
       }
      steps{
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle/jar/"
       }

  }
    stage('docker'){
      agent{
        docker  'openjdk:8u151-jre-alpine'
        }
      steps{
        sh "wget http://ec2-18-218-59-2.us-east-2.compute.amazonaws.com/rectangle/jar/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 5"
        }
       }
    stage('promote to blue'){
      agent{
        label 'app'
           }
      steps{
        sh cp /var/www/html/rectangle/jar/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle/blue/rectangle_${env.BUILD_NUMBER}.jar
         }
      }
}
}
