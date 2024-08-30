pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building the project using Maven...'
                // Tool: Maven
                // Task: Compile and package the code into an executable format.
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Tool: JUnit for Unit Tests, Selenium for Integration Tests
                // Task: Ensure the code functions correctly and different components work together.
                sh 'mvn test'
            }
            post {
                always {
                    script {
                        sendEmailNotification('Unit and Integration Tests')
                    }
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code quality with SonarQube...'
                // Tool: SonarQube
                // Task: Analyze the code for code quality, bugs, and vulnerabilities.
                sh 'mvn sonar:sonar'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Performing security scan using OWASP Dependency Check...'
                // Tool: OWASP Dependency Check
                // Task: Identify vulnerabilities in project dependencies.
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
            post {
                always {
                    script {
                        sendEmailNotification('Security Scan')
                    }
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying application to the staging environment...'
                // Tool: Ansible or Jenkins SSH plugin for deployment
                // Task: Deploy the packaged application to a staging server (e.g., AWS EC2).
                sh 'ansible-playbook -i staging_inventory deploy.yml'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on the staging environment...'
                // Tool: Selenium or Cucumber
                // Task: Validate the application in a production-like environment.
                sh 'mvn verify -Pstaging'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying application to the production environment...'
                // Tool: Ansible or Jenkins SSH plugin for deployment
                // Task: Deploy the application to the production server.
                sh 'ansible-playbook -i production_inventory deploy.yml'
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

def sendEmailNotification(stageName) {
    mail to: 'developer@example.com',
         subject: "${stageName} Stage: ${currentBuild.currentResult}",
         body: "The ${stageName} stage completed with status: ${currentBuild.currentResult}.",
         attachLog: true
}
