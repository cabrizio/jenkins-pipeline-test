pipeline {
    agent {  label 'master' }
    stages {
        stage('Build') {
            steps {
                sh """
                sudo yum install git -y
                sudo yum update -y && yum upgrade -y
                sudo ifconfig
                echo ${JOB_NAME}
                echo ${BRANCH_NAME}
                """
            }
        }
         stage('record build env') {
            steps{
                sh '/usr/sbin/ifconfig > ifconfig-env.txt'
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
                                        execCommand: 'sudo cat /opt/jenkins_file/ifconfig-env.txt && sudo yum update -y'
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

    

