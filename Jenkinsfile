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
                // Sử dụng đường dẫn đầy đủ đến mvn.cmd để đảm bảo Jenkins tìm thấy Maven
                bat 'C:\\apache-maven-3.9.11\\bin\\mvn.cmd clean install -DskipTests'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                // Sử dụng đường dẫn đầy đủ đến mvn.cmd để đảm bảo Jenkins tìm thấy Maven
                bat 'C:\\apache-maven-3.9.11\\bin\\mvn.cmd test'
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
