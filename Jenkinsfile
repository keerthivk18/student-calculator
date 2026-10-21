
pipeline {
    agent any

    parameters {
        booleanParam(
            name: 'RUN_APP',
            defaultValue: true,
            description: 'Run application'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source checked out by Jenkins SCM'
            }
        }

        stage('Compile') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: 'target/surefire-reports/TEST-*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar',
                                 fingerprint: true
            }
        }

        stage('Run') {
            when {
                expression { params.RUN_APP }
            }
            steps {
                echo 'RUN_APP is true: Run stage selected.'
            }
        }
    }
}
