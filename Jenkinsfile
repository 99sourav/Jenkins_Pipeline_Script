pipeline {
    agent any

    environment {
        MAVEN_HOME = "C:/Users/addre/Downloads/apache-maven-3.9.9-bin/apache-maven-3.9.9"    // Adjust this path to your Maven installation
        TOMCAT_HOME = "C:/apache-tomcat-9.0.98"    // Adjust this path to your Tomcat installation
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code from GitHub...'
                git branch: 'master', url: 'https://github.com/99sourav/sample_java_project.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Maven project...'
                bat "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Apache Tomcat...'
                script {
                    // Stop Tomcat server
                    bat "${TOMCAT_HOME}/bin/shutdown.sh"

                    // Remove old WAR and application directory
                    bat "rm -rf ${TOMCAT_HOME}/webapps/springwebapp"
                    bat "rm -f ${TOMCAT_HOME}/webapps/springwebapp.war"

                    // Copy the new WAR file
                    bat "cp target/springwebapp.war ${TOMCAT_HOME}/webapps/"

                    // Start Tomcat server
                    bat "${TOMCAT_HOME}/bin/startup.sh"
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment completed successfully!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
