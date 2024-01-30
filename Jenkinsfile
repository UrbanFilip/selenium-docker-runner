pipeline {
  agent any

  parameters {
  choice choices: ['chrome', 'firefox'], description: 'Select browser', name: 'BROWSER'
}

  stages {

      stage('Start grid') {
        steps {
         bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
        }
      }

      stage('Run Test') {
        steps{
          bat "docker-compose -f test-suites.yaml up --pull=always"
          script {
            if(fileExists('output/file-reservation/testng-failed.xml') || fileExists('output/file-reservation/testng-failed.xml')) {
              error('failed tests found')
            }
          }
        }
      }
  }
  post {
    always {
      bat "docker-compose -f grid.yaml down"
      bat "docker-compose -f test-suites.yaml down"
      archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
      archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
    }
  }
}