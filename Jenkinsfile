pipeline{
    agent{
        node{
            label 'maven'
        }
    }
    stages{
        stage('Checkout from SCM'){
            steps{
                git branch: 'main', url: 'https://github.com/paycenonoli/ovia-ravdy-devops.git'
            }
        }
    }
}