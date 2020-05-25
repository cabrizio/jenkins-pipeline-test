def remote = [:]
remote.name = "test"
remote.host = "192.168.9.97"
remote.allowAnyHosts = true

pipeline {
    agent {  label 'master' }
    stages {
        stage('Build') {
            steps {
                sh """
                sudo yum install git -y
                sudo yum update -y && sudo yum upgrade -y
                echo ${JOB_NAME}
                echo ${BRANCH_NAME}
                """
            }
        }
         stage('record build env') {
            steps{
                sh 'sudo yum update -y > yum-env.txt'
                archiveArtifacts artifacts: 'yum-env.txt', fingerprint: true
        }
     }
         stage ('Decide auto deployment') {
            steps {
                script {
                    if ( env.BRANCH_NAME == 'develop/QA') {
                        env.DEPLOY_PACKAGE = input message: 'Admin permission required',
                        parameters: [choice(name: 'To be Deployed', choices: 'no\nyes', description: 'Choose "yes" if you want to deploy this build')]
                    } else {
                        echo 'Deployment decision not required'
                    }
                }
            }
        }
        stage ('DeployToTest') {
            steps {
                script {
                    if (env.DEPLOY_PACKAGE == 'yes') {
                     sshagent (credentials: ['jenkins_root_key']) {
                      sh 'ssh -o StrictHostKeyChecking=no -l root 192.168.9.97 uname -a'
                       }
                     }
                 }
                } else {
                        echo 'QA Deploy not required'
                    }
                }
            }
        }
    }
}

    

