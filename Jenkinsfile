def parameterFile
 if (BRANCH_NAME == 'develop/qa') {
    parameterFile = ".env.qa"
 } else if (BRANCH_NAME == 'develop/staging') {
    parameterFileLux = ".env.stage"
    parameterFileAsia = ".env.asia.stage"
 } else if (BRANCH_NAME == 'develop/production') {
    parameterFileLux = ".env.luxembourg"
    parameterFileAsia = ".env.asia"
 } else {
    parameterFile = ".env.qa"
 }

pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Build - Copy ENV file') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'develop/staging') {
                        sh "echo ${parameterFile}"
                    } else if (env.BRANCH_NAME == 'develop/production') {
                        sh "echo ${parameterFile}"
                    } else {
                        echo 'QA ENV file'
                        sh "echo ${parameterFile}"
                    }
                }
            }
        }
    }
}
