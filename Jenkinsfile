pipeline{
    agent any
    tools{
        maven 'maven-3.5.2'
        jdk 'LOCAL_JAVA'
        
    }
    stages{
        stage('First stage'){
            steps{
                echo "Working fine"
        }
        }
        stage('Second stage'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[credentialsId: 'fdb9e62a-d878-4bc2-92b7-cc5c50610bf3', url: 'https://github.com/suresh2316/maven-project.git']]])

        }
        }
        stage('SonarQube analysis') {
            steps{
                withSonarQubeEnv('SonarQube') {
                // requires SonarQube Scanner for Maven 3.2+
                    bat 'mvn clean package sonar:sonar'
            }
            }
        }
        stage('Quality Gate') {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
            post{
                success{
                    archiveArtifacts '**/*.war'        
                }
            }
          }
        stage('Copying Artifacts'){
            steps{
            copyArtifacts filter: '**/target/*.war', fingerprintArtifacts: true, projectName: 'DecPipeline', target: 'F:\\ExcelFilesforJava'
        }
        }
        }
}
