pipeline {
    agent any
    environment {
        JAVA_HOME = '/Users/wajdibenrabah/Library/Java/JavaVirtualMachines/openjdk-22.0.1/Contents/Home'
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
        ANDROID_HOME = '/Users/wajdibenrabah/Library/Android/sdk'
    }
    stages {
        stage('Environment Check') {
            steps {
                sh 'java -version'
                sh 'echo $JAVA_HOME'
                sh 'echo $ANDROID_HOME'
                sh './gradlew --version'
            }
        }
        stage('Setup') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('SAST Analysis') {
            steps {
                sh 'python3 /chemin/vers/android_sast_analyzer.py /chemin/vers/votre/apk'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew assembleDebug --stacktrace'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test --stacktrace'
            }
        }
    }
}