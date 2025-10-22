pipeline {
    agent any

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('STUDENTAPI-REACT') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                    # Remove old frontend files
                    sudo rm -rf /var/lib/tomcat10/webapps/reactstudentapi
                    sudo mkdir -p /var/lib/tomcat10/webapps/reactstudentapi

                    # Copy new build files
                    sudo cp -r STUDENTAPI-REACT/dist/* /var/lib/tomcat10/webapps/reactstudentapi/
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('STUDENTAPI-SPRINGBOOT') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                    # Remove old WAR & exploded app
                    sudo rm -rf /var/lib/tomcat10/webapps/springbootstudentapi*
                    
                    # Copy new WAR file
                    sudo cp STUDENTAPI-SPRINGBOOT/target/*.war /var/lib/tomcat10/webapps/springbootstudentapi.war
                '''
            }
        }

    }

    post {
        success {
            echo ' Deployment Successful!'
        }
        failure {
            echo ' Pipeline Failed.'
        }
    }
}
