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
                // Thay đổi thư mục làm việc vào thư mục 'java-main/backend-auth' nơi chứa pom.xml
                dir('java-main/backend-auth') { // <--- ĐÃ CẬP NHẬT ĐƯỜNG DẪN
                    bat 'C:\\apache-maven-3.9.11\\bin\\mvn.cmd clean install -DskipTests'
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                // Thay đổi thư mục làm việc vào thư mục 'java-main/backend-auth' nơi chứa pom.xml
                dir('java-main/backend-auth') { // <--- ĐÃ CẬP NHẬT ĐƯỜNG DẪN
                    bat 'C:\\apache-maven-3.9.11\\bin\\mvn.cmd test'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                // Nếu Dockerfile và docker-compose.yml cũng nằm trong 'java-main/backend-auth', bạn cũng cần dir vào đó
                dir('java-main/backend-auth') { // <--- THÊM DÒNG NÀY NẾU DOCKERFILE/COMPOSE CŨNG Ở ĐÂY
                    bat 'docker-compose build'
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo 'Stopping existing containers...'
                // Nếu Dockerfile và docker-compose.yml cũng nằm trong 'java-main/backend-auth', bạn cũng cần dir vào đó
                dir('java-main/backend-auth') { // <--- THÊM DÒNG NÀY NẾU DOCKERFILE/COMPOSE CŨNG Ở ĐÂY
                    bat 'docker-compose down'
                    echo 'Starting up containers...'
                    bat 'docker-compose up -d'
                }
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
