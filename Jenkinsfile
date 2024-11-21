pipeline {
    agent {
        label 'jenkins-docker-agent'  // Use the label of the dynamic Docker agent
    }
    options {
        // This is required if you want to clean before build
        skipDefaultCheckout(true)
    }
    stages {
        stage("clone") {
            steps {
                cleanWs()
                git branch: 'main', url: 'https://github.com/Mallikarjun-S-H/addressbook.git'
                echo "cloning...."
            }
        }
        stage('Build') {
            steps {
                
                sh 'mvn -B -DskipTests clean package'
                sh 'chmod -R 777 target'
                sh 'ls /workspace/doja-u-pipeline/target'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
            }
        }
        stage('Deploy') {
            steps{
            script {
                sshagent(['deploy-server-key']) {
                
                echo "Deploying to Tomcat..."
                     sh 'chmod -R 777 /opt'
                    // Stop Tomcat before deployment (if necessary)
            
                     // Copy the WAR file to Tomcat's webapps directory
                sh '''
                scp -o StrictHostKeyChecking=no ${WORKSPACE}/target/addressbook.war ubuntu@13.232.100.77:/opt/tomcat/webapps/
                 '''
            
                // Start Tomcat after deployment
            
                 // Verify deployment
            sh '''
                echo "Deployment completed. Checking deployed files:"
             
            '''
}
                }
            }
        }
        
    }
    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs() // Clean workspace after the job completes
        }
    }
}

