pipeline {
    agent any

    tools {
        jdk 'jdk21'
    }

    environment {
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {

        stage('Install Dependencies') {
            steps {
                bat '"C:/Users/EL__Mehdi/AppData/Local/Programs/Python/Python310/python.exe" -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat '"C:/Users/EL__Mehdi/AppData/Local/Programs/Python/Python310/python.exe" -m pytest --junitxml=pytest-report.xml'
            }
        }

        stage('SAST Scan - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat "\"%SCANNER_HOME%\\bin\\sonar-scanner.bat\""
                }
            }
        }

        stage('SCA Scan - Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan . --format XML --format HTML --out . --failOnCVSS 7', odcInstallation: 'dependency-check'
            }
        }

        stage('Publish Reports') {
            steps {
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
    }

    post {
        always {
            junit 'pytest-report.xml'
        }
    }
}