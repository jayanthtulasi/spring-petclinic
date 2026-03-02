pipeline {
    agent { label 'JAVA_SPC' }

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

        stage('build & scan') {
            steps {
                withCredentials([string(credentialsId: 'my_sonar', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            mvn package sonar:sonar \
                            -Dsonar.projectKey=jayanthtulasi_spring-petclinic \
                            -Dsonar.organization=jayanthtulasi \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
        //  stage("Quality Gate") {
        //     steps {
        //       timeout(time: 1, unit: 'MINUTES') {
        //         waitForQualityGate abortPipeline: true     
        //       }
        //     }
        //   }
    }
    post {
        always{
            archiveArtifacts artifacts: '**/*.jar'
            junit '**/surefire-reports/*.xml'

        }
    }
}