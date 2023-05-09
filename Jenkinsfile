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
     stage('mvn-package') {
            steps {
                sh 'mvn package'
		sh 'mv target/*.war target/tomcat-demo.war'
            }
        }
     stage('Tomcat-deployment') {
            steps {
                sshagent(['tomcat-credentials']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/tomcat-demo.war 
			ec2-user@44:204:177:252:/opt/tomcat/webapps/  
			
			ssh ec2-user@44:204:177:252 /opt/tomcat/bin/shutdown.sh
			ssh ec2-user@44:204:177:252 /opt/tomcat/bin/startup.sh
                    """
            }
        }
    }
   } 
 } 
