pipeline {
  agent any
  tools {
        maven 'maven'
        jdk 'jdk8'
  }
  stages {
    stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    echo "JAVA_HOME= ${JAVA_HOME}"
                '''
       }
    }
    stage('Unit Test') {
      steps {
      withMaven(maven: 'maven', mavenSettingsConfig: 'c462e880-e0d1-4c31-bef5-1db5a8571773') {
      sh '''
          echo "JAVA_HOME= ${JAVA_HOME}"
          mvn clean test
      '''
        }
      }
    }
    stage('Deploy CloudHub') {
      environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
        muleEnv = "${env.cloudhub_env.toLowerCase()}"
      }
      steps {
        echo "----Publishing to Artifactory------ "
        withMaven(maven: 'maven', mavenSettingsConfig: 'c462e880-e0d1-4c31-bef5-1db5a8571773'){
          sh 'mvn -e clean -DskipMunitTests deploy'
        }
        echo "----Running Build ${env.BUILD_ID} on muleEnv - ${muleEnv}----- "
        sh 'mvn clean -DskipMunitTests deploy -DmuleDeploy'
      }
    }
  }
}
