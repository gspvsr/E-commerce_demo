pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        DOCKER_IMAGE = 'gspvsr/ecommerce:latest'
    }

    stages {
        stage ('git checkout') {
            steps {
                git 'https://github.com/gspvsr/E-commerce_demo.git' 
            }
        }

        stage ('maven complie') {
            steps {
                sh "mvn compile"
            }
        }

        stage ('maven test') {
            steps {
                sh "mvn test -DskipTests=true"
            }
        }

        stage ('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
    }




}