pipeline {
    agent any

    environment {
        JAR_NAME = "smartcontactmanager-0.0.1-SNAPSHOT.jar"
        DEPLOY_DIR = "${WORKSPACE}"
        SERVER_PORT = "8291"
        JAVA_PATH = "C:\\Program Files\\Java\\jdk-17.0.4.1\\bin\\java.exe"
    }

    stages {
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
                        cd target
                        echo start java -jar ${JAR_NAME} > start.bat
                        echo Start.bat created successfully.
                    """

                    echo 'Starting the application using start command...'
                    bat """
                        cd ${DEPLOY_DIR}
                        cd target
                        start cmd /k java -jar ${JAR_NAME}
                    """
                     echo 'Application started.'
                    
                }
            }
        }

    }
}
