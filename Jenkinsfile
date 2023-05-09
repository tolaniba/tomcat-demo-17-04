pipeline {
    agent any
    tools {
        maven 'mvn_home'
    }
    stages {
        stage('SCM-Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-credentials', url: 'https://github.com/tolaniba/tomcat-demo-17-04.git'
            }
        }
    stage('build') {
            steps {
                sh 'mvn compile'
            }
        }
    stage('Sonar-testing') {
            steps {
              withSonarQubeEnv('sonar-server') {
                sh 'mvn sonar:sonar'
              }
            }
        }
     stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
               }
            }
          }
     stage('package') {
            steps {
                sh 'mvn clean compile package'
		
            }
        }
     stage('Tomcat-deployment') {
            steps {
                echo 'Tomcat-Deployment'
        }
    }
   } 
 } 
