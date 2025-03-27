pipeline {
    agent any

    environment {
        JAR_NAME = "smartcontactmanager-0.0.1-SNAPSHOT.jar"
        DEPLOY_DIR = "${WORKSPACE}/target"
        SERVER_PORT = "8291"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main', url: 'https://github.com/shivajipawar1928/smart-contacts.git'
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building application using Maven Wrapper...'
                bat './mvnw.cmd  package'
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
            echo 'Creating start.bat to deploy in background...'
            bat """
                echo java -jar ${JAR_NAME} --server.port=${SERVER_PORT} > start.bat
                echo exit >> start.bat
                start /B start.bat
            """
            echo "Application started using start.bat. Logs are in app.log"
        }
    }
}





    }
}
