pipeline {
    agent any
      environment {
            HARNESS_API_KEY = credentials('harness-api-key')
            HARNESS_ACCOUNT = credentials('harness-account')
    }
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
                def response = httpRequest httpMode: 'POST', url: 'https://app.harness.io/gateway/v1/catalog/custom-properties',
                    customHeaders: [[name: 'Harness-Account', value: $HARNESS_ACCOUNT],
                                    [name: 'x-api-key', value: $HARNESS_API_KEY]],
                    contentType: 'APPLICATION_JSON',
                    requestBody: '''{
                                "properties": [
                                            {
                                                "field": "metadata.test_coverage_score",
                                                "filter": {
                                                    "kind": "Component",
                                                    "type": "service",
                                                    "tags": [
                                                        "nkenenbayev"
                                                    ]
                                                },
                                                "value": "1"
                                            }
                                        ]
                                }'''
                println("Status: ${response.status}")
                println("Response: ${response.content}")
                println("Headers: ${response.headers}")
            }
        }

    }
}
