pipeline {
    agent any

    environment {
        // Define environment variables
        MAVEN_HOME = '/usr/share/maven'
        JAVA_HOME = '/usr/lib/jvm/java-17-amazon-corretto'
        TOMCAT_HOME = '/opt/tomcat'
        TOMCAT_PORT = '9090'
        WAR_FILE = 'target/NumberGuessGame-1.0-SNAPSHOT.war'
        DEPLOY_DIR = "${TOMCAT_HOME}/webapps"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the repository
                git branch: 'main', url: 'https://github.com/Imma016/NumberGuessGame.git'
            }
        }

        stage('Build') {
            steps {
                // Run Maven build
                script {
                    sh "mvn clean package"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy WAR file to Tomcat
                    sh "sudo cp target/NumberGuessGame-1.0-SNAPSHOT.war /opt/tomcat/webapps/"
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    // Restart Tomcat to ensure the application is deployed
                    sh """
                    sudo -S /opt/tomcat/bin/shutdown.sh
                    sudo -S /opt/tomcat/bin/startup.sh
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    // Introduce a short delay to give the application time to start
            sleep(time: 10, unit: 'SECONDS')
                    // Verify that the app is running by making a simple HTTP request
                    def response = sh(script: "curl -s -o /dev/null -w \"%{http_code}\" http://44.223.48.56:9090/NumberGuessGame-1.0-SNAPSHOT/", returnStdout: true).trim()
                    if (response == "200") {
                        echo "Application deployed successfully!"
                    } else {
                        error "Application deployment failed!"
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up and stop Tomcat if needed
            echo 'Pipeline finished'
        }

        success {
            echo 'Deployment completed successfully!'
        }

        failure {
            echo 'Deployment failed'
        }
    }
}

