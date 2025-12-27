pipeline {
    agent none

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        USER_NAME = "vhazarathnaidu"
    }

    stages {

        stage('Initialize') {
            agent any
            steps {
                echo " Initializing pipeline..."
                script {
                    def branchName = env.BRANCH_NAME ?: env.GIT_BRANCH ?: 'main'
                    env.CLEAN_BRANCH = branchName.replaceFirst(/^origin\//, '')
                    echo " Detected Git Branch: ${env.CLEAN_BRANCH}"
                }
            }
        }

        stage('Checkout') {
            agent { label 'java' }
            steps {
                echo " Checking out source code on Java agent..."
                git(
                    url: 'https://github.com/vhazarathnaidu/java-source.git',
                    branch: "${env.CLEAN_BRANCH}"
                )
                echo " Checkout completed successfully"
            }
        }

        stage('Build') {
            agent { label 'java' }
            steps {
                dir('java'){
                echo " Starting Maven build on Java agent..."
                sh '''
                    echo " Verifying tools"
                    java -version
                    mvn -v

                    echo " Running Maven build"
                    mvn clean package
                '''
                echo " Build completed successfully"
            }
        }
        }

        stage('Test') {
            agent { label 'java' }
            steps {
                dir('java'){
                echo " Running tests on Java agent..."
                sh 'mvn test'
                echo " Tests completed successfully"
            }
        }
    }
    }

    post {
        success {
            node('java') {
            echo " Build SUCCESS on Java agent for branch: ${env.CLEAN_BRANCH}"
            echo "Archiving build artifacts..."
            archiveArtifacts artifacts: '**/target/*.jar', 
            junit '**/target/surefire-reports/*.xml'
            echo " Artifacts archived successfully"
            echo "publish the test-reports.."
           
        }
        }

        failure {
            echo "Build FAILED on Java agent for branch: ${env.CLEAN_BRANCH}"
        }

        always {
            echo "Pipeline execution finished"
        }
    }
}

