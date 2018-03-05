pipeline {
  agent {
    label 'master'
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
  }
  stages {
    stage('Unit test'){
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('build'){
      steps {
        sh 'ant -f build.xml -v'
      }
    }
    stage('deploy'){
      steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar  /usr/share/nginx/html/rectangles/all/"
      }
    }
  }
  post {
    always {
       archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}
