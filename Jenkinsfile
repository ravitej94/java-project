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
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle/jar/${env.BRANCH_NAME}/"
       }

  }
    stage('docker'){
      agent{
        docker  'openjdk:8u151-jre-alpine'
        }
      steps{
        sh "wget http://ec2-18-216-180-183.us-east-2.compute.amazonaws.com/rectangle/jar/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 5"
        }
       }
    stage('promote to blue'){
      agent{
        label 'app'
           }
      when{
        branch 'master'
          }
      steps{
        sh "cp /var/www/html/rectangle/jar/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangle/blue/rectangle_${env.BUILD_NUMBER}.jar"
         }
      }
    stage('promote to master')
       {
      agent{
        label 'app'
          }
       when{
        branch 'development'
      }
       steps{
         echo "git stash"
         sh 'git stash'
         echo "Devlopment checkout"
         sh 'git checkout development'
         echo "Master checkout"
         sh 'git checkout master'
         echo "merging dev"
         sh 'git merge development'
         echo "Push"
         sh 'git push origin master'
       }
       }
}
}
