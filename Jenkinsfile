pipeline {
  agent none

  options {
    buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
  }
  stages {
    stage('Unit test'){
      agent {
        label 'master'
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build'){
      agent {
        label 'master'
      }
      steps {
        sh 'ant -f build.xml -v'
      }
      post {
        success {
         archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }
    stage('deploy'){
      agent {
        label 'master'
      }
      steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar  /usr/share/nginx/html/rectangles/all/"
      }
    }
   stage("Running on centos") {
     agent {
       label 'CentoS'
     }
     steps {
       sh "wget http://34.218.78.48/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
       sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
     }
   }
   stage("TEst on Debian") {
    agent {
      label 'master'
      docker 'openjdk:8u151-jre' 
    }
    steps {
      sh "wget http://34.218.78.48/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
      sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
    }
   }
  }
}
