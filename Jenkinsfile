pipeline {
    agent none   
	
    triggers {
        githubPush()
		pollSCM('* * * * *')
    }

    environment {
        USER_NAME = "vhazarathnaidu"
        BRANCH_NAME = "${env.GIT_BRANCH}".replace('origin/', 'feature-ep-01-task-01')
    }

    stages {

        stage('Parallel Build') {
            parallel {

                stage('Java Repo Build') {
                    agent { label 'Java' }

                    stages {
                        stage('Checkout') {
                            steps {
							echo "checkout to Java repo..."
							echo "${env.USER_NAME}"
							echo "${env.BRANCH_NAME}"
                                git(
                                    url: "https://github.com/vhazarathnaidu/java-source.git",
                                    branch: "feature-ep-01-task-01"
                                )
                            }
                        }

                        stage('Build Java') {
                            steps {
                                dir("java") {
                                    script {
                                        if (isUnix()) {
										echo "Compailing Java program..."
                                            sh "javac *.java"
                                        } else {
										echo "Compailing Java program..."
                                            bat "javac *.java"
                                        }
                                    }
                                }
                            }
                        }

                        stage('Run Java') {
                            steps {
                                dir("java") {
                                    script {
                                        if (isUnix()) {
										echo "Running Java program..."
                                            sh "java Main"
                                        } else {
										echo "Running Java program..."
                                            bat "java Main"
                                        }
                                    }
                                }
                            }
                        }
                    }
                }


                stage('Python Repo Build') {
                    agent { label 'Python' }

                    stages {
                        stage('Checkout Python Repo') {
                            steps {
							echo "checkout to python repo..."
							echo "${env.USER_NAME}"
							echo "${env.BRANCH_NAME}"							
                                git(
                                    url: "https://github.com/vhazarathnaidu/python-source.git",
                                    branch: "feature-ep-01-task-01"
                                )
                            }
                        }

                        stage('Run Python') {
                            steps {
                                dir("python") {
                                    script {
                                        if (isUnix()) {
										echo "Running Python program..."
                                            sh "python Hello.py"
                                        } else {
										echo "Running python program"
                                            bat "python Hello.py"
                                        }
                                    }
                                }
                            }
                        }
                    }
                }


                stage('Node Repo Build') {
                    agent { label 'Nodejs' }

                    stages {
                        stage('Checkout Node Repo') {
                            steps {
							echo "checkout to node repo..."
							echo "${env.USER_NAME}"
							echo "${env.BRANCH_NAME}"
                                git(
                                    url: "https://github.com/vhazarathnaidu/node-source.git",
                                    branch: "feature-ep-01-task-01"
                                )
                            }
                        }

                        stage('Run Node App') {
                            steps {
                                dir("node") {
                                    script {
                                        if (isUnix()) {
										echo "Running Nodejs program..."
                                            sh "node Hello.js"
                                        } else {
										echo "Running Nodejs program..."
                                            bat "node Hello.js"
                                        }
                                    }
                                }
                            }
                        }
                    }
                }

            } 
        }
    }
}