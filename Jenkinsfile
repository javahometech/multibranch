pipeline{
    agent { label 'master'}
    tools {
        maven 'maven3'
    }
    stages{
        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }
    }
}
