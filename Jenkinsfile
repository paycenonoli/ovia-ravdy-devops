pipeline{
    agent{
        node{
            label 'maven'
        }
    }
    environment{
        PATH = "/opt/apache-maven-3.9.7/bin:$PATH"
    }
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean deploy'
            }
        }

        stage('Unit Test'){
            steps{
                echo "------- Unit Test"
                sh 'mvn surefire-report:report'
            }
        }
        stage('SonarQube analysis') {
        environment { 
            scannerHome = tool 'ovia-sonar-scanner'
        }
            steps{
                withSonarQubeEnv('ovia-sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
                sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}

