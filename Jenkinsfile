pipeline {
    agent { label 'jdk11-mvn3.8.6' }
    triggers { 
        cron('45 23 * * 1-5')
        pollSCM('*/5 * * * *')
    }
    environment {
        CI = true
        ARTIFACTORY_ACCESS_TOKEN = credentials('JFROG-ARTIFACTORY')
    }
    stages {
        stage('scm') {
            steps {

                git url: 'https://github.com/GitPracticeRepo/spring-petclinic.git', branch: 'main'
            }
        }
        stage('build') {
            steps {
                withSonarQubeEnv(installationName: 'SONAR_8.9.10', envOnly: true, credentialsId: 'SONAR_TOKEN') {
                    sh "/usr/local/apache-maven-3.8.6/bin/mvn clean package sonar:sonar"
					echo "${env.SONAR_HOST_URL}"

                }

            }
        }
    }

}