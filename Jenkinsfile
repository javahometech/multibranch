pipeline{
    agent { label 'master'}
    tools {
        maven 'maven3'
    }
    stages{
        stage('Maven Build'){
            when {
                branch 'develop'
            }
            steps{
                sh "mvn clean package"
            }
        }
        stage('Upload to Nexus'){
            when {
                branch 'develop'
            }
            steps{
                echo "coming soon.."
            }
        }

        stage('Deploy to Dev'){
            when {
                branch 'develop'
            }
            steps{
                echo "coming soon.."
            }
        }

        stage('Deploy to QA'){
            when {
                branch 'release'
            }
            steps{
                echo "coming soon.."
            }
        }

        stage('Deploy to Prod'){
            when {
                branch 'master'
            }
            steps{
                echo "coming soon.."
            }
        }

    }
}
