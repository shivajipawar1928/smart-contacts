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
            echo 'Deploying application using the generated .jar file...'
            bat """
                cd ${DEPLOY_DIR}
                echo Starting application...
                start /B java -jar ${JAR_NAME} --server.port=${SERVER_PORT} > app.log 2>&1
            """
            echo "Application started on port ${SERVER_PORT}. Logs are saved to app.log"
        }
    }
}



    }
}
