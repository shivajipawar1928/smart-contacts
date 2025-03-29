pipeline {
    agent any

    environment {
        JAR_NAME = "smartcontactmanager-0.0.1-SNAPSHOT.jar"
        DEPLOY_DIR = "${WORKSPACE}"
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
            catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                def isTargetEmpty = bat(script: 'dir target /a /b | find /v /c ""', returnStdout: true).trim()
                if (isTargetEmpty != '0') {
                    echo "Target folder is not empty, running clean."
                    bat './mvnw.cmd clean'
                } else {
                    echo "Target folder is empty, skipping clean."
                }
            }
        }
    }
}

stage('Build') {
    steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
            echo "Building the project..."
            bat './mvnw.cmd package'
        }
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
                        cd target
                        echo cmd /k start java -jar ${JAR_NAME} --server.port=${SERVER_PORT} > start.bat
                        echo Start.bat created successfully.
                    """

                    echo 'Starting the application using start command...'
                    bat """
                        cd ${DEPLOY_DIR}
                        cd target
                        start.bat
                        echo Application started.
                    """
                    
                }
            }
        }

    }
}
