pipeline {
  
  agent any
  stages {
    stage('Preparacion') {
      steps {
        options { 
			    buildDiscarder(logRotator(numToKeepStr: '0')) 
			  }
        git(url: 'https://desreposrv.goldcar.es:8443/scm/ic/${REPOSITORIO}.git', branch: '*/${RAMA}')
      }
    }
    stage('Ejecucion') {
      steps {
        sh '''gradle clean
              gradle clearDB
              gradle build
              gradle jacocoTestReport
              gradle install
              gradle uploadArchives
              '''
      }
    }
    stage('Tratamiento') {
      steps {
        cobertura(fileCoverageTargets: '**/**.exec')
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/test-results/test/*.xml', healthScaleFactor: 1)
      }
    }
  }
}
