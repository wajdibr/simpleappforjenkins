pipeline {
    agent any
    environment {
        JAVA_HOME = '/Users/wajdibenrabah/Library/Java/JavaVirtualMachines/openjdk-22.0.1/Contents/Home'
        ANDROID_HOME = '/Users/wajdibenrabah/Library/Android/sdk'
        PATH = "${env.JAVA_HOME}/bin:${env.ANDROID_HOME}/platform-tools:/opt/homebrew/bin:${env.PATH}"
        VENV_PATH = "${WORKSPACE}/venv"

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
                sh '''
                python3 -m venv ${VENV_PATH}
                . ${VENV_PATH}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('SAST Analysis') {
            steps {
                sh '''
                    . ${VENV_PATH}/bin/activate
                    echo $PATH
                    which jadx || echo "jadx not found"
                    jadx --version || echo "jadx not working"
                    python3 /Users/wajdibenrabah/Documents/projects/sec-project/src/android/android_sast_analyzer.py /Users/wajdibenrabah/Documents/projects/sec-project/src/android/fcc.apk
                '''
                archiveArtifacts artifacts: 'sast_report.json', allowEmptyArchive: true
            }
        }
        stage('Display Results') {
            steps {
                sh 'cat sast_report.json'
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