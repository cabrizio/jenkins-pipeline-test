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
                    if (env.DEPLOY_PACKAGE == 'yes' && env.DEPLOY_ENV == 'staging') {
                      withCredentials([sshUserPrivateKey(credentialsId: 'sshUser', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                        remote.user = root
                        remote.identityFile = identity
                        stage("SSH Steps Rocks!") {
                         writeFile file: 'abc.sh', text: 'ls'
                         sshCommand remote: remote, command: 'for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done'
                         sshPut remote: remote, from: 'abc.sh', into: '.'
                         sshGet remote: remote, from: 'abc.sh', into: 'bac.sh', override: true
                         sshScript remote: remote, script: 'abc.sh'
                         sshRemove remote: remote, path: 'abc.sh'
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

    

