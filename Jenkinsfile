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
                      withCredentials([usernamePassword(credentialsId: 'jenkins_root_key', usernameVariable: 'USERNAME')]) {
                       sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'test',
                                sshCredentials: [
                                    username: "$USERNAME"
                                    //encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'yum-env.txt',
                                        //removePrefix: '.',
                                        remoteDirectory: 'jenkins_file/',
                                        execCommand: 'sudo cat /opt/jenkins_file/yum-env.txt && sudo yum update -y'
                                    )
                                ]
                            )
                        ]
                    )
                   }
               } else {
                        echo 'QA Deploy not required'
                    }
                }
            }
        }
    }
}

    

