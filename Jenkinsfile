pipeline {
    agent any

    tools {
        maven 'maven-3.8.1'    // Match this with your Maven tool name
        jdk 'jdk-17'           // Match this with your JDK tool name
    }

    environment {
        SONARQUBE = 'LocalSonar'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Checking out source code...'
                git branch: 'master',
                    url: 'https://github.com/YashwanthKumar49/java-maven-app.git'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building the application...'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running Unit Tests...'
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo '📦 Packaging the application...'
                sh 'mvn package'
            }
        }

        stage('Static Code Analysis (Checkstyle)') {
            steps {
                echo '🔍 Running Checkstyle analysis...'
                sh 'mvn checkstyle:checkstyle'
                recordIssues tools: [checkStyle()]
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '🧠 Running SonarQube analysis...'
                withSonarQubeEnv("${env.SONARQUBE}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }

    post {
        success {
            echo '✅ CI Pipeline completed successfully!'
        }
        failure {
            echo '❌ CI Pipeline failed. Check logs for details.'
        }
    }
}
