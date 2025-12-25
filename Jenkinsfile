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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                    $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=ECommerce \
                    -Dsonar.projectKey=ECommerce \
                    -Dsonar.java.binaries=target/classes
                    '''
                }
            }
        }
        stage('Maven Build') {
            steps {
                sh "mvn package -DskipTests=true"
            }
        }
        stage('Publish to Nexus') {
            steps {
                withMaven(globalMavenSettingsConfig: 'mven-setting', jdk: 'jdk17', maven: 'maven') {
                    sh "mvn deploy -DskipTests=true"
                }
            }
        }
        stage('Docker Build & Tag') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub-cred') {
                        sh "docker build -t ${DOCKER_IMAGE} ."
                    }
                }
            }
        }
    }




}