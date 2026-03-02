pipeline {
    agent { label 'JAVA_SPC' }

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/jayanthtulasi/spring-petclinic.git',
                    branch: 'main'
            }
        }

        stage('Build & Scan') {
            steps {
                withCredentials([string(credentialsId: 'my_sonar', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=jayanthtulasi_spring-petclinic \
                            -Dsonar.organization=jayanthtulasi \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
    }
}