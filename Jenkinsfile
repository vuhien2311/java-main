pipeline {
    agent any

    environment {
        MAVEN_OPTS = "-Dmaven.repo.local=.m2/repository"
    }

    stages {
        stage('Configure Git Safe Directory') {
            steps {
                echo "Adding Jenkins workspace to Git's safe directories..."
                bat 'git config --global --add safe.directory "%WORKSPACE%"'
            }
        }

        stage('Checkout Code') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build Project with Maven') {
            steps {
                echo 'Running mvn clean install...'
                // Dùng mvn nếu Jenkins đã cài Maven global, hoặc ./mvnw nếu có wrapper
                bat 'mvn clean install -DskipTests'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                bat 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                bat 'docker-compose build'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo 'Stopping existing containers...'
                bat 'docker-compose down'

                echo 'Starting up containers...'
                bat 'docker-compose up -d'
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
        success {
            echo '✅ Build and deployment succeeded.'
        }
        failure {
            echo '❌ Build or deployment failed.'
        }
    }
}
