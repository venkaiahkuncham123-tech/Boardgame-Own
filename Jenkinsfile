pipeline {
    agent any
    tools {
        jdk 'venkaiah-jdk'
        maven 'venkaiah-maven'
    }
    environment {
      SONAR_HOME = tool 'venkaiah-sonar-scanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
               git 'https://github.com/venkaiahkuncham123-tech/Boardgame-Own.git'
            }
        }
        stage('Comiplation and build, test') {
            steps {
                sh 'mvn compile'
                sh 'mvn test'
            }
        }
        stage('Sonar Qube Analysis') {
            steps {
                withSonarQubeEnv('venkaiah-sonar') {
                    sh '''$SONAR_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=Boardgame \
                    -Dsonar.projectKey=Boardgame \
                    -Dsonar.java.binaries=.
                    '''
                 }
            }
        }
        stage('Building Application') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Deploying to Nexus') {
            steps {
              withMaven(globalMavenSettingsConfig: '072047e4-5147-48c8-9124-165ddbe5cd51', jdk: 'venkaiah-jdk', maven: 'venkaiah-maven', traceability: true) {
                       sh 'mvn deploy'
                }               
            }
        }        
    }
    post {
            success {
                sh 'echo Application Deployed to Nexus'
            }
            failure {
                sh 'echo Error'
            }
        }
}
