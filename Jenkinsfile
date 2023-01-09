pipeline {
  agent any
  tools {
        maven 'maven'
        jdk 'jdk8'
  }
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('connectedAppClientCredentials')
    MULE_VERSION = '4.4.0'
    BG = "NitinSurana-BG"
    REGION="us-east-2"
    WORKER = "MICRO"
    // M2SETTINGS = "/Users/nsurana/.m2/settings.xml"
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
    stage('Deploy CloudHub Sandbox') {
      environment {
        ENVIRONMENT = "${env.cloudhub_env}"
      }
      steps {
        echo "----Running Build ${env.BUILD_ID} on muleEnv - ${ENVIRONMENT}----- "
        withMaven(maven: 'maven', mavenSettingsConfig: 'c462e880-e0d1-4c31-bef5-1db5a8571773'){
          sh 'mvnDebug clean -DskipMunitTests deploy -DmuleDeploy'
        }
      }
    }
  }
}
