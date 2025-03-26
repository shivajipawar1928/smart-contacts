pipeline {
    agent any

    environment {
        JAR_NAME = "smartcontactmanager-0.0.1-SNAPSHOT.jar"
        DEPLOY_DIR = "C:\Users\Shree\Documents\workspace-spring-tool-suite-4-4.20.1.RELEASE\smartcontactmanager\target" // Choose a directory to deploy the app
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/shivajipawar1928/smart-contacts.git' // Add your repo URL
            }
        }

        stage('Build Jar') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Stop Existing Application') {
            steps {
                script {
                    sh """
                        echo "Checking for running application..."
                        PID=\$(lsof -ti :8291)
                        if [ ! -z "\$PID" ]; then
                            echo "Stopping existing application with PID: \$PID"
                            kill -9 \$PID
                        else
                            echo "No application running on port 8291."
                        fi
                    """
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    sh """
                        echo "Copying jar to deployment directory..."
                        mkdir -p ${C:\Users\Shree\Documents\workspace-spring-tool-suite-4-4.20.1.RELEASE\smartcontactmanager\target}
                        cp target/${smartcontactmanager-0.0.1-SNAPSHOT.jar} ${C:\Users\Shree\Documents\workspace-spring-tool-suite-4-4.20.1.RELEASE\smartcontactmanager\target}

                        echo "Starting application..."
                        nohup java -jar ${C:\Users\Shree\Documents\workspace-spring-tool-suite-4-4.20.1.RELEASE\smartcontactmanager\target}/${smartcontactmanager-0.0.1-SNAPSHOT.jar} > ${C:\Users\Shree\Documents\workspace-spring-tool-suite-4-4.20.1.RELEASE\smartcontactmanager\target}/app.log 2>&1 &
                        echo "Application started. Logs are available in ${C:\Users\Shree\Documents\workspace-spring-tool-suite-4-4.20.1.RELEASE\smartcontactmanager\target}/app.log"
                    """
                }
            }
        }
    }
}
