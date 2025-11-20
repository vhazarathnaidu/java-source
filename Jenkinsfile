pipeline {
    agent any

    triggers {
        githubPush()
        pollSCM('H/2 * * * *')
        
    }

    environment {
        USER_NAME = "vhazarathnaidu"
		current_branch= "${env.GIT_BRANCH}"
        BRANCH_NAME= "${env.GIT_BRANCH}".replace('origin/', '')
    }

    stages {
	stage('Pre-Build: Create Jenkins Agents') {
      
    steps {
        script {
            echo "creating agents using scriptfile..."
           if(isUnix()){
		   sh 'agents.sh'
		   }else{
		   bat 'D:\\sfts\\Agents\\agents.bat'
		   }

        }
    }
}
      stage('wait for agents') {
    steps {
        script {
            timeout(time: 2, unit: 'MINUTES') {

                def agents = ['java-agent', 'python-agent', 'nodejs-agent']

                for (nodeLabel in agents) {
                    echo "waiting for ${nodeLabel} to come online..."

                    waitUntil {
                        try {
                            node(nodeLabel) {
                                echo "${nodeLabel} is online!"
                            }
                            return true
                        } catch (Exception e) {
                            return false
                        }
                    }
                }
            }
        }
    }
}


        stage('Java Build') {
            agent { label 'java' }

            stages {
                stage('Checkout Java Repo') {
                    steps {
					 cleanWs()
                         echo "Checkout to Java repo..."
						 echo "Current branch name: ${env.BRANCH_NAME}"
				         echo "Current branch name: ${env.GIT_BRANCH}" 
				         echo "username: ${env.USER_NAME}"
                       
                        git(
                            url: "https://github.com/vhazarathnaidu/java-source.git",
                            branch: "${env.BRANCH_NAME}"
                        )
                    }
                }

                stage('Build Java') {
                    steps {
					dir("java"){
                        script {
                            if (isUnix()) {
                                echo "Compiling Java program on Linux..."
                                sh """
								javac Hello.java
								javac Main.java
								"""
                            } else {
                                echo "Compiling Java program on Windows..."
                                bat "javac Hello.java"
								bat "javac Main.java"
                            }
                        }
                    }
					}
                }

                stage('deploy Java') {
                    steps {
					dir("java"){
                        script {
                            if (isUnix()) {
                                echo "Running Java program on Linux..."
                                sh """
                                java Main
                                java Hello
                                """
                            } else {
                                echo "Running Java program on Windows..."
                                bat "java Main"
                                bat "java Hello"
                            }
                        }
                    }
					}
                }
            }
        }

        stage('python') {
            agent { label 'python' }

            stages {
                stage('Checkout Python Repo') {
                    steps {
					 cleanWs()
                        echo "Checkout to Python repo..."
						echo "Current branch name: ${env.BRANCH_NAME}"
				        echo "Current branch name: ${env.GIT_BRANCH}" 
				        echo "username: ${env.USER_NAME}"
                     
                        git(
                            url: "https://github.com/vhazarathnaidu/python-source.git",
                            branch: "${env.BRANCH_NAME}"
                        )
                    }
                }

                stage('Run Python') {
                    steps {
					dir("python"){
                        script {
                            if (isUnix()) {
                                echo "Running Python script on Linux..."
                                sh "python Hello.py"
                            } else {
                                echo "Running Python script on Windows..."
                                bat "python Hello.py"
                            }
                        }
                    }
					}
                }
            }
        }

        stage('nodejs') {
            agent { label 'nodejs' }

            stages {
                stage('Checkout Nodejs Repo') {
                    steps {
					 cleanWs()
                        echo "Checkout to Nodejs repo..."
						echo "Current branch name: ${env.BRANCH_NAME}"
				        echo "Current branch name: ${env.GIT_BRANCH}" 
				        echo "username: ${env.USER_NAME}"
                        
                        git(
                            url: "https://github.com/vhazarathnaidu/node-source.git",
                            branch: "${env.BRANCH_NAME}"
                        )
                    }
                }

                stage('Run Nodejs') {
                    steps {
					dir("node"){
                        script {
                            if (isUnix()) {
                                echo "Running Node.js on Linux..."
                                sh "node Hello.js"
                            } else {
                                echo "Running Node.js on Windows..."
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
