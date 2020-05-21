def agentLabel
 if (BRANCH_NAME == 'deploy/QA') {
    agentLabel = "deploy"
 } else if (BRANCH_NAME == 'deploy/staging') {
    agentLabel = "STAGE-NJ01"
 } else if (BRANCH_NAME == 'deploy/production') {
    agentLabel = "PROD-NJ01"
 } else {
    agentLabel = "master"
 }


pipeline {
    agent { node { label agentLabel } }
    environment {
        SEC_JOB_NAME = env.JOB_NAME.replaceFirst('%2F', '/')
    }
    stages {
        stage('Build') {
            steps {
                sh 'ifconfig'
                sh 'hostname'
                sh 'echo ${JOB_NAME}'
            }
        }
         stage('record build env') {
    		steps{
                sh 'ifconfig > ifconfig-env.txt'
                archiveArtifacts artifacts: 'ifconfig-env.txt', fingerprint: true
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
