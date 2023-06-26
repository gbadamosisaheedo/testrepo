pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/ken4class/testrepo'
            }
        }
        
        // stage("SonarQube analysis") {
        //    steps {
        //      withSonarQubeEnv('sonarqube') {
        //          sh 'mvn clean package sonar:sonar'
        //      }
        //    }
        // }
        
        // stage('Quality Gate') {
        //     steps {
        //         waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
        //     }
        // }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Publish Test report') {
            steps {
                junit 'target/surefire-reports/TEST-com.example.mywebapp.RegisterServletTest.xml'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'RegistrationApp',
               classifier: '', file: 'target/RegistrationApp-*.war',
               type: 'war']], 
               credentialsId: 'nexus', 
               groupId: 'com.example', 
               nexusUrl: '3.88.130.147:8081', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
               repository: 'my-repo',
               version: 'v1.$GIT_COMMIT'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
               deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://54.90.95.130:8080')], contextPath: 'demo', war: 'target/RegistrationApp-*.war'
            }
        }
    }
}