pipeline {
    agent any
    tools {
        maven 'Maven'
   }
    stages {
        stage('Build') {
            steps { 
                snDevOpsStep()
                echo 'Build' 
                sh '''
                    export M2_HOME=/opt/apache-maven-3.6.0 # your Mavan home path
                    export PATH=$PATH:$M2_HOME/bin
                    mvn --version
                '''
                sh 'mvn compile' 
            }         
        }
        stage('Test') {
            steps { 
                snDevOpsStep()
                echo 'Test'
                sh '''
                    export M2_HOME=/opt/apache-maven-3.6.0 # your Mavan home path
                    export PATH=$PATH:$M2_HOME/bin
                    mvn --version
                '''
                sh 'mvn verify'
            }
            post {
                success {
                    junit '**/target/surefire-reports/*.xml' 
                }
            }       
        }
        stage('Publish') {
            steps { 
                snDevOpsStep()
                echo "${env.BUILD_NUMBER}"
                snDevOpsArtifact(artifactsPayload:"""{"artifacts": [{"name": "rapdev-web.war","version":"2.${env.BUILD_NUMBER}.0","semanticVersion": "2.${env.BUILD_NUMBER}.0","repositoryName": "RapDev-Demo2"}]}""")
                echo 'Publish' 
            }         
        }
        stage('UAT') {
            steps { 
                snDevOpsStep()
                echo 'UAT' 
            }         
        }
        stage('Package') {
            steps { 
                snDevOpsStep()
                snDevOpsPackage(name: "package", artifactsPayload: """{"artifacts": [{"name": "rapdev-web.war", "repositoryName": "RapDev-Demo2", "version": "2.${env.BUILD_NUMBER}.0"}]}""")
                echo 'Package' 
            }         
        }
        stage('Prod') {
            steps { 
                snDevOpsStep()
                snDevOpsChange()
                echo 'PROD' 
            }         
        }
    }
}
