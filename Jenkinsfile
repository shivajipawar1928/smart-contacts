pipeline {
    agent any

    environment {
        JAR_NAME = "smartcontactmanager-0.0.1-SNAPSHOT.jar"
        DEPLOY_DIR = "${WORKSPACE}/target"
        SERVER_PORT = "8291"
        JAVA_PATH = "C:\\Program Files\\Java\\jdk-17.0.4.1\\bin\\java.exe"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main', url: 'https://github.com/shivajipawar1928/smart-contacts.git'
            }
        }

       stage('Clean') {
            steps {
                script {
                    def isEmpty = bat(script: 'dir target /a /b | find /v /c "" || exit 0', returnStdout: true).trim()
                    if (isEmpty == '0') {
                        echo "Target folder is empty, skipping clean step."
                    } else {
                        echo "Target folder is not empty, running clean."
                        bat './mvnw.cmd clean'
                    }
                }
            }
        }

         stage('Build') {
            steps {
                bat './mvnw.cmd package'
            }
        }
        
        stage('Stop Existing Application') {
            steps {
                script {
                    echo 'Checking if an application is running on port 8291...'
                    def result = bat(script: 'netstat -ano | findstr :8291', returnStatus: true)
                    if (result == 0) {
                        echo 'Application detected. Attempting to stop...'
                        def output = bat(script: 'netstat -ano | findstr :8291', returnStdout: true).trim()
                        def pid = output.split()[-1]
                        echo "Stopping application with PID: ${pid}"
                        bat "taskkill /F /PID ${pid}"
                    } else {
                        echo "No application found on port 8291."
                    }
                }
            }
        }

       stage('Deploy Application') {
            steps {
                script {
                    echo 'Creating start.bat for application deployment...'
                    bat """
                        cd ${DEPLOY_DIR}
                        echo java -jar ${JAR_NAME} --server.port=${SERVER_PORT} > start.bat
                        echo Start.bat created successfully.
                    """

                    echo 'Starting the application using start command...'
                    bat """
                        cd ${DEPLOY_DIR}
                        start cmd /k start.bat
                    """
                    echo 'Application started successfully.'
                }
            }
        }

    }
}
