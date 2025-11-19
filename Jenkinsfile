pipeline {
    agent none

    triggers {
        githubPush()
        pollSCM('* * * * *')
    }

    environment {
        USER_NAME = "vhazarathnaidu"
        BRANCH_NAME = "${env.GIT_BRANCH}".replace('origin/', '')
    }

    stages {
        stage('Parallel Builds') {
            parallel {
                stage('Checkout') {
                   
                    steps {
                        echo "Current branch name: ${env.BRANCH_NAME}"
                        echo "Current branch name: ${env.GIT_BRANCH}" 
                        echo "username: ${env.USER_NAME}"
                        git (
                            url: "https://github.com/vhazarathnaidu/java-source.git",
                            branch: "${env.BRANCH_NAME}"
                        )
                    }
                }

                stage('Build Java Project') {
                    agent { label: 'Java' }
                    steps {
                        dir("java") {
                            script {
                                if (isUnix()) {
                                    sh """ 
                                        echo "Compiling Java program..."
                                        javac Hello.java
                                        javac Main.java
                                    """
                                } else {
                                    bat "javac Hello.java"
                                    bat "javac Main.java"
                                }
                            }
                        }
                    }
                }

                stage('Deploy') {
                    agent { label: 'Java' }
                    steps {
                        dir("java") {
                            script {
                                if (isUnix()) {
                                    sh """ 
                                        echo "Running Java program in unix shell ..."
                                        java --version
                                        java Main
                                        echo "Completed ..."
                                    """
                                } else {
                                    echo "Running Java program in windows shell..."
                                    bat "java Hello"
                                    bat "java Main"
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
