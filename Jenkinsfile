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
                    bat """
                        echo Starting application...
                        cd ${DEPLOY_DIR}
                        cd target
                        java -jar smartcontactmanager-0.0.1-SNAPSHOT.jar --server.port=8291 > application.log 2>&1
                        """
                     echo 'Application started.'
                    
                }
            }
        }

    }
}
