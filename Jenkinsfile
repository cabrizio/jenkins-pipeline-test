pipeline {
    agent any
    environment {
        JOB_NAME_new = env.JOB_NAME.replaceFirst('%2F', '/')
    }
    stages {
        stage('Build') {
            steps {
                sh 'ifconfig'
                sh 'hostname'
                sh 'echo ${JOB_NAME_new}'
            }
        }
    }
}

