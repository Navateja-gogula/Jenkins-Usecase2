pipeline {
    agent any

    environment {
        PYTHON_PATH = 'C:\\Users\\Admin-BL\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
        PYTHON_SCRIPT_LOAD = 'scripts\\load_csv_to_gcp.py'
        PYTHON_SCRIPT_SUMMARY = 'scripts\\generate_summary.py'
        SUMMARY_FILE = 'upload_summary.txt'
        EMAIL_RECIPIENT = 'gogulanavateja10@gmail.com'
    }

    stages {
        stage('Checkout Repository') {
            steps {
                git url: 'https://github.com/Navateja-gogula/Jenkins-Usecase2.git', branch: 'main'
            }
        }

        stage('Setup Python and Dependencies') {
            steps {
                bat """
                    "${env.PYTHON_PATH}" -m pip install --upgrade pip setuptools wheel
                    "${env.PYTHON_PATH}" -c "import pandas" || "${env.PYTHON_PATH}" -m pip install pandas
                    "${env.PYTHON_PATH}" -c "import pyodbc" || "${env.PYTHON_PATH}" -m pip install pyodbc
                """
            }
        }

        stage('Load CSV to GCP') {
            steps {
                bat """
                    "${env.PYTHON_PATH}" "${env.PYTHON_SCRIPT_LOAD}"
                """
            }
        }

        stage('Generate Summary Report') {
            steps {
                bat """
                    "${env.PYTHON_PATH}" "${env.PYTHON_SCRIPT_SUMMARY}"
                """
            }
        }

        stage('Email Summary') {
            steps {
                emailext (
                    subject: "✅ Jenkins Pipeline Test Email",
                    body: "This is a test email sent from Jenkins pipeline using emailext.",
                    to: "gogulanavateja10@gmail.com",
                    from: "gogulateja92@gmail.com",
                    replyTo: "gogulateja92@gmail.com",
                    mimeType: 'text/plain'
                )
            }
        }
    }

    post {
        failure {
            emailext (
                subject: "❌ GCP Upload Pipeline FAILED",
                body: """\
The Jenkins job has failed.

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
""",
                to: "gogulanavateja10@gmail.com",
                from: 'gogulateja92@gmail.com',
                replyTo: "gogulateja92@gmail.com"
            )
        }
    }
}
