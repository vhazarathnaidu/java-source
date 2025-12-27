pipeline {
 agent none
triggers {
        githubPush()
        pollSCM('H/5 * * * *')
    }

 environment {
        USER_NAME = "vhazarathnaidu"
        BRANCH_NAME = "${env.GIT_BRANCH}".replace('origin/', '')
    }
stages {
    stage('checkout') {
          agent { label 'java' }
		echo "checking out the code from branch: ${BRANCH_NAME}"
        echo "Current Branch: ${current_branch}"
        echo "User Name: ${USER_NAME}"
        steps {
            git (
                url: 'https://github.com/vhazarathnaidu/java-source.git',
                branch: "${BRANCH_NAME}"
            )
        }
	}
    stage('Build') {
        agent { label 'java' }
        steps {
            echo 'Building..'
            sh 'mvn clean package'
        }
    }
    stage('Test') {
        agent { label 'java' }
        steps {
            echo 'Testing..'
            sh 'mvn test'
        }
    }
    } 
post {
   always {
    echo " archiving the artifacts"
    archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
    junit '**/target/surefire-reports/*.xml'
    }
    success {
         script {
                def branchBuild = env.BRANCH_NAME ?: env.GIT_BRANCH

                echo "BUILD SUCCESS DETAILS"
                echo "Job Name     : ${env.JOB_NAME}"
                echo "Build ID     : ${env.BUILD_ID}"
                echo "Build Number : ${env.BUILD_NUMBER}"
                echo "Build URL    : ${env.BUILD_URL}"
                echo "Branch Built : ${branchBuilt}"
            }
        
    }
  }
}

       
        
