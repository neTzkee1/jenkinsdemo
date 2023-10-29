pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                echo 'Building project'
                build 'build-job'
            }
        }
        stage('SonarQube analysis') {
          steps {
            script {
              scannerHome = tool 'sonarscanner'
            }
            withSonarQubeEnv('sonarqube') {
              bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=pkey"
            }
            jacoco()
          }
        }
        stage("deploy"){
            steps {
                echo WORKSPACE
                copyArtifacts fingerprintArtifacts: true, projectName: 'build-job', selector: lastSuccessful(), target: 'target'
                deploy adapters: [tomcat9(credentialsId: 'ea1a9011-9cc9-453d-a814-5a76b3193d4e', path: '', url: 'http://localhost:8090')], contextPath: 'jenkinsdemo-1.0-SNAPSHOT', war: '**/*.war'
            }
        }
    }
}
