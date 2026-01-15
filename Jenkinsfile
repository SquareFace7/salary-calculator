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
                // ג'נקינס מושך את הקוד אוטומטית, אבל זה שלב פורמלי
                checkout scm
            }
        }

        stage('Run Script') {
            steps {
                // הרצת הסקריפט עם הפרמטרים שהוזנו
                // שים לב: אנחנו מעבירים את המשתנים שהוגדרו למעלה
                sh "python3 salary_calc.py \"${params.EMPLOYEE_NAME}\" ${params.HOURLY_RATE} ${params.HOURS_WORKED} ${params.IS_HOLIDAY} \"${params.DATE}\""
            }
        }
    }

    post {
        always {
            // שמירת הדוח גם אם יש שגיאה
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'salary_report.html', // וודא שזה השם שהפייתון שלך מייצר!
                reportName: 'Salary Report',
                reportTitles: 'Salary Report'
            ])
        }
    }
}
