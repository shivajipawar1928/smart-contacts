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
                script {
                    echo 'Checking if a previous build is there...'
                    def result = bat(script: './mvnw.cmd clean', returnStatus: true)
                    if (result == 0) {
                        echo 'Build detected. Attempting to clean...'
                        
                        echo "Cleaning previous build"
                        bat "./mvnw.cmd clean"
                    } else {
                        echo "No previous build."
                    }
                }
                
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
            echo 'Deploying application using the generated .jar file...'
            bat """
                cd ${DEPLOY_DIR}
                echo Starting application using cmd /c...
                cmd /c start "" "C:\\Program Files\\Java\\jdk-17.0.4.1\\bin\\java.exe" -jar ${JAR_NAME}
            """
            echo "Application started in the background on port ${SERVER_PORT}."
        }
    }
}












    }
}
