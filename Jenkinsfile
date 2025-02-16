pipeline {

    agent any

    environment {
                registry = "borhencherif/cicd"
                registryCredential = "dockerhub"

    }

    stages {
       
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

        stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
        
         stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
        
        stage('CODE ANALYSIS with SONARQUBE') {

            environment {
                scannerHome = tool 'Sonar_4.6'
            }

            steps {
                withSonarQubeEnv('Sonar_Server') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=devops_pipeline \
                   -Dsonar.projectName=devops_pipeline \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
               
            }
        }

      }
 
 }






