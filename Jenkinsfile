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
                echo "------- Build Started-------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "------- Build Completed-------"
            }
        }

        stage('Unit Test'){
            steps{
                echo "------- Unit Test Started-------"
                sh 'mvn surefire-report:report'
                echo "------- Unit Test Completed-------"
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

        stage("Quality Gate"){
            steps{
                script{
                    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                        def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                        if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }    
            }    
        }
    }
}

