pipeline {
    agent any

    environment {
        JAR_NAME = "smartcontactmanager-0.0.1-SNAPSHOT.jar"
        APP_NAME = "SpringBootApp"
        DEPLOY_DIR = "http://192.168.43.138" // Local or Remote Path
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/shivajipawar1928/smart-contacts.git'
            }
        }

        stage('Build Jar') {
            steps {
                sh './mvnw clean package' // Use mvnw if using wrapper, else mvn
            }
        }

        stage('Deploy Jar') {
            steps {
                script {
                    def jarPath = "target/${smartcontactmanager-0.0.1-SNAPSHOT.jar}"
                    sh """
                        echo "Stopping existing application..."
                        pkill -f ${smartcontactmanager-0.0.1-SNAPSHOT.jar} || echo "No existing app found."

                        echo "Copying jar to deploy directory..."
                        cp ${target/smartcontactmanager-0.0.1-SNAPSHOT.jar} ${http://192.168.43.138}

                        echo "Starting new application..."
                        nohup java -jar ${http://192.168.43.138}/${smartcontactmanager-0.0.1-SNAPSHOT.jar} > ${http://192.168.43.138}/app.log 2>&1 &
                        echo "Application started successfully."
                    """
                }
            }
        }
    }
}
