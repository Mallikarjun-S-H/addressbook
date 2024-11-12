pipeline {
    agent { label 'slave2-u' }

    stages {
        stage('gitclone') {
            steps {
                echo "cloning..."
                git branch: 'main', url: 'https://github.com/Mallikarjun-S-H/addressbook.git'
            }
        }
        stage('build') {
            steps {
                echo "build..."
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                echo "testing..."
                sh 'mvn test'
            }
        }
         stage('deploy') {
            steps {
                echo "packaging..."
                sh 'mvn package'
            }
        }
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', allowEmptyArchive: true
            }
        }
    }
}
