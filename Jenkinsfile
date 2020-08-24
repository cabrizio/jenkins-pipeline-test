pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Build - Test') {
            steps {
                echo 'Create Artifact'
                script {
                    sh 'ping www.google.com -c 5'
                }
            }
        }
    }
}
