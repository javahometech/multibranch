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
        stage("Sonar Analysis"){
            steps{
              withSonarQubeEnv('sonar7') {
                 sh 'mvn clean package sonar:sonar'
              }
              timeout(time: 1, unit: 'HOURS') {
                   def qg = waitForQualityGate()
                   if (qg.status != 'OK') {
                       error "Pipeline aborted due to quality gate failure: ${qg.status}"
                   }
               }
                
            }
        }
        stage('Upload to Nexus'){
            when {
                branch 'develop'
            }
            steps{
                script{
                    def pom = readMavenPom file: 'pom.xml'
                    def repository = pom.version.endsWith('SNAPSHOT') ? 'javahome-app-snapshot' : 'javahome-app'
                    nexusArtifactUploader artifacts: [
                        [artifactId: 'multibranch', classifier: '', file: 'target/multibranch.war', type: 'war']
                    ], 
                    credentialsId: 'nexus3', 
                    groupId: pom.groupId, 
                    nexusUrl: '172.31.70.16:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: repository, 
                    version: pom.version
                }
            }
        }

        stage('Deploy to Dev'){
            when {
                branch 'develop'
            }
            steps{
                sshagent(['dev-tomcat']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.80.188 /opt/tomcat8/bin/shutdown.sh"
                    sh "scp target/multibranch.war ec2-user@172.31.80.188:/opt/tomcat8/webapps/"
                    sh "ssh ec2-user@172.31.80.188 /opt/tomcat8/bin/startup.sh"
                }
            }
        }

        stage('Deploy to QA'){
            when {
                branch 'release'
            }
            steps{
                sshagent(['dev-tomcat']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.89.39 /opt/tomcat8/bin/shutdown.sh"
                    sh "scp target/multibranch.war ec2-user@172.31.89.39:/opt/tomcat8/webapps/"
                    sh "ssh ec2-user@172.31.89.39 /opt/tomcat8/bin/startup.sh"
                }
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
