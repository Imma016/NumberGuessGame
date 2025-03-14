pipeline {
    agent any

    environment {
        TOMCAT_WEBAPPS = "/opt/tomcat/webapps"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Imma016/NumberGuessGame.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                sh '''
                    # Stop Tomcat before deployment
                    sudo /opt/tomcat/bin/shutdown.sh || true
                    
                    # Remove old deployment
                    sudo rm -rf $TOMCAT_WEBAPPS/NumberGuessGame*
                    
                    # Copy new WAR file
                    sudo cp target/NumberGuessGame.war $TOMCAT_WEBAPPS/
                    
                    # Start Tomcat
                    sudo /opt/tomcat/bin/startup.sh
                '''
            }
        }
    }
}
