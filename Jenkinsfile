pipeline {
    agent any

    environment {
        JAR_NAME = "my-app.jar"
        APP_NAME = "SpringBootApp"
        DEPLOY_DIR = "/path/to/deploy" // Local or Remote Path
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/username/your-repo.git'
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
                    def jarPath = "target/${JAR_NAME}"
                    sh """
                        echo "Stopping existing application..."
                        pkill -f ${JAR_NAME} || echo "No existing app found."

                        echo "Copying jar to deploy directory..."
                        cp ${jarPath} ${DEPLOY_DIR}

                        echo "Starting new application..."
                        nohup java -jar ${DEPLOY_DIR}/${JAR_NAME} > ${DEPLOY_DIR}/app.log 2>&1 &
                        echo "Application started successfully."
                    """
                }
            }
        }
    }
}
