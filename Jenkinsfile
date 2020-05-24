pipeline {
    agent {  label 'master' }
    environment {
        //SEC_JOB_NAME = env.JOB_NAME.replaceFirst('%2F', '/')
	//BRANCH_NAME_new = '${BRANCH_NAME}'
    }
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
                                        removePrefix: '.',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo cat /tmp/ifconfig-env.txt'
                                    )
                                ]
                            )
                        ]
                    )
		}
	    }
             post{
                always{
                    step([  $class: 'CopyArtifact',
                            filter: 'ifconfig-env.txt',
                            flatten: true,
                            fingerprintArtifacts: true,
                            projectName: '${JOB_NAME}',
                            selector: [
                            $class: 'SpecificBuildSelector',
                            buildNumber: '${BUILD_NUMBER}'
                            ] ])
		}
	     }
	 }
    }
}
