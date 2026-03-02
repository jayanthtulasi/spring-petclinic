pipeline {
    agent {label 'JAVA_SPC'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('git checkout') {
            steps {
                git url: 'https://github.com/jayanthtulasi/spring-petclinic.git',
                    branch: 'main' 
            }
        }
        stage('build and scan') {
            steps {
             withCredentials([string(credentialsId: 'my_sonar', variable: 'SONAR_TOKEN')]) {   
             withSonarQubeEnv('SonarQube') {   
                sh """ mvn package sonar:sonar \
                       -Dsonar.projectKey= jayanthtulasi_spring-petclinic \
                       -Dsonar.organization=jayanthtulasi \
                       -Dsonar.host.url=https://sonarcloud.io/ \
                       -Dsonar.login=$SONAR_TOKEN """ 
            }
          }  
        }
     } 
    
    }  
  }