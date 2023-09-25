pipeline {
    agent { label 'JenkinSlave-1' }

    triggers {
        pollSCM '* * * * *'
    }

    stages {
        stage('SCM Checkout') {
            steps {
                // Checkout code from a Git repository
                checkout([$class: 'GitSCM', branches: [[name: '*/feat01']], userRemoteConfigs: [[url: 'https://github.com/InfoFusion-Society/mavenrepo.git']]])
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('SonarQube') {
            steps {
                // Run SonarQube analysis
                withSonarQubeEnv('sonarscanner') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Nexus') {
            steps {
                sh 'mvn deploy'
            }
        }
        stage('TomCat') {
            steps {
                sh 'scp /jenkins-slave-worksplace/workspace/build/feature/target/studentapp-2.1.1-FEAT01-SNAPSHOT.war root@172.31.38.202:/opt/apache-tomcat-9.0.80/webapps'
            }
        }
  
    }
    
        
    post {
        always {
            // Send Slack notification on both success and failure
            slackSend(channel: 'slack-notification', message: 'Build 1.0.0 finished!')
        }
    }
}


