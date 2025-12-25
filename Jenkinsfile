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
            git 'https://github.com/gspvsr/E-commerce_demo.git' 
        }
    }




}