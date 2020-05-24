pipeline {
    agent {  label 'master' }
    stages {
        stage('Build') {
            steps {
                sh 'ifconfig'
                sh 'hostname'
                sh 'echo ${JOB_NAME}'
                sh 'echo ${BRANCH_NAME}'
        sh 'echo $BRANCH_NAME_new'
            }
        }
         stage('record build env') {
            steps{
                sh 'ifconfig > ifconfig-env.txt'
                archiveArtifacts artifacts: 'ifconfig-env.txt', fingerprint: true
        }
     }
     stage('DeployToTest') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'jenkins_user', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'test',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'ifconfig-env.txt',
                                        //removePrefix: '.',
                                        remoteDirectory: 'jenkins_file/',
                                        execCommand: 'sudo cat jenkins_file/ifconfig-env.txt'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}

    

