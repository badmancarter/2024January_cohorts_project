pipeline {
    agent any

    stages {   
        stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
        }

        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
        
        stage('Quality Code Scan Analysis') {
           steps {
                  withSonarQubeEnv('sonar_server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"     
               }
            }
       }

        
        stage('Push to Nexus Artifactory') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'sonar_ID_NEW2', groupId: 'SampleWebApp', nexusUrl: '54.92.191.139:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
         deploy adapters: [tomcat9(credentialsId: 'deployTomcatnew2', path: '', url: 'http://54.198.149.135:8080/')], contextPath: 'App', war: '**/*.war'
             
            }
            
        }
            
    }
} 
