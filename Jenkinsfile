pipeline {
    agent any

    parameters {
        string(name: 'EMPLOYEE_NAME', defaultValue: 'Eliad', description: 'Name of the employee')
        string(name: 'HOURLY_RATE', defaultValue: '50', description: 'Hourly rate')
        string(name: 'HOURS_WORKED', defaultValue: '10', description: 'Hours worked')
        booleanParam(name: 'IS_HOLIDAY', defaultValue: false, description: 'Is it a holiday?')
        string(name: 'DATE', defaultValue: '01/01/2026', description: 'Date of calculation')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Script') {
            steps {
                // הרצת הסקריפט
                sh "python3 salary_calc.py \"${params.EMPLOYEE_NAME}\" ${params.HOURLY_RATE} ${params.HOURS_WORKED} ${params.IS_HOLIDAY} \"${params.DATE}\""
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'salary_slip.html', // תיקנתי את השם כאן!
                reportName: 'Salary Report',
                reportTitles: 'Salary Slip'
            ])
        }
    }
}
