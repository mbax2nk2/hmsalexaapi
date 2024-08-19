pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh './gradlew clean build' // Use 'mvn clean install' if using Maven
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test' // Use 'mvn test' if using Maven
            }
        }
        stage('Package') {
            steps {
                sh './gradlew bootJar' // Use 'mvn package' if using Maven
            }
        }
    }

    post {
        success {
            echo 'Build and Deploy succeeded!'
        }
        failure {
            echo 'Build or Deploy failed!'
        }
        always {
            junit 'build/test-results/**/*.xml'
            script {
                echo "harness"
            }
        }

    }
}
