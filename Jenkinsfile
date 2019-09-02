pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        
        stage('Sonarqube') {
            environment {
            scannerHome = tool 'MySonarQubeScanner'
            }    
           
            steps {
                withSonarQubeEnv('MySonarQube') {
                sh "/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/MySonarQubeScanner/bin/sonar-scanner"
                }        
                timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
                }
           }
        }
        
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
}
