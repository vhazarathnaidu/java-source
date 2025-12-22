pipeline {
    agent any

    triggers {
        githubPush()
        pollSCM('H/5 * * * *')
    }

    environment {
        USER_NAME = "vhazarathnaidu"
        BRANCH_NAME = "${env.GIT_BRANCH}".replace('origin/', '')
    }

    stages {
        stage('Checkout Java Repo') {
            agent { label 'java' }
            steps {
                cleanWs() // clean only at the start
                echo "Checking out Java repo..."
                git(
                    url: "https://github.com/vhazarathnaidu/java-source.git",
                    branch: "${env.BRANCH_NAME}"
                )
            }
        }

       
        stage('Build Java') {
            agent { label 'java' }
            steps {
                dir('java') {
                    script {
                        if (isUnix()) {
                            echo "Compiling Java files..."
                            sh 'javac *.java'
                        } else {
                            echo "Compiling Java files on Windows..."
                            bat 'javac *.java'
                        }
                    }
                }
            }
        }

        stage('deploy Java') {
            agent { label 'java' }
            steps {
                dir('java') {
                    script {
                        if (isUnix()) {
                            echo "Running Java programs..."
                            sh 'java Main'
                            sh 'java Hello'
                        } else {
                            echo "Running Java programs on Windows..."
                            bat 'java Main'
                            bat 'java Hello'
                        }
                    }
                }
            }
        }

        stage('deploy python') {
            agent { label 'python' }
            steps {
                cleanWs()
                echo "Checking out Python repo..."
                git(
                    url: "https://github.com/vhazarathnaidu/python-source.git",
                    branch: "${env.BRANCH_NAME}"
                )

                dir('python') {
                    script {
                        if (isUnix()) {
                            echo "Running Python script..."
                            sh 'python3 Hello.py'
                        } else {
                            bat 'python Hello.py'
                        }
                    }
                }
            }
        }

        stage('deploy nodejs') {
            agent { label 'nodejs' }
            steps {
                cleanWs()
                echo "Checking out Node.js repo..."
                git(
                    url: "https://github.com/vhazarathnaidu/node-source.git",
                    branch: "${env.BRANCH_NAME}"
                )

                dir('node') {
                    script {
                        if (isUnix()) {
                            echo "Running Node.js script..."
                            sh 'node Hello.js'
                        } else {
                            bat 'node Hello.js'
                        }
                    }
                }
            }
        }

    }
}

