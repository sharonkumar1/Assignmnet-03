pipeline {
  agent any

  tools {
    maven 'Maven'   // matches the name in Jenkins Global Tool Config
    jdk   'JDK21'   // matches your JDK21 entry
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
      post {
        success {
          archiveArtifacts artifacts: 'target/*.war', fingerprint: true
        }
      }
    }

    stage('Deploy to Tomcat') {
      steps {
        deploy adapters: [tomcat9(
          credentialsId: 'tomcat-admin',
          url: 'http://localhost:8090/manager/text'
        )],
        contextPath: '/my-webapp',
        war: 'target/my-webapp.war'
      }
    }
  }
}
