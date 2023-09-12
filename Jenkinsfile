pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git branch: "${params.branchName}", credentialsId: 'github-tokens', url: 'https://github.com/Manmadharao123/hr-api'
            }
        }
        stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Tomcat Deploy - Dev') {
            steps {
                sshagent(['tomcat-dev']) {
                    sh "scp -o StrictHostKeyChecking=no target/hr-api.war ec2-user@10.0.3.246:/opt/tomcat9/webapps/"
                    sh "ssh ec2-user@10.0.3.246 /opt/tomcat9/bin/shutdown.sh"
                    sh "ssh ec2-user@10.0.3.246 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
    post {
  regression {
    cleanWs()
  }
}
}
