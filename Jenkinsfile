pipeline {
    agent {
        // המרצה ביקש בחירת Agent לפי פרמטר
        label "${params.RUN_ON_NODE}"
    }

    parameters {
        // הוספנו בחירה בין built-in (הלינוקס הראשי) לבין windows (המחשב שלך)
        choice(name: 'RUN_ON_NODE', choices: ['built-in', 'windows'], description: 'Select where to run the job')
        
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
                script {
                    // בדיקה: אם אנחנו בלינוקס נריץ פקודת לינוקס, אם בווינדוס נריץ פקודת ווינדוס
                    if (isUnix()) {
                        echo "Running on Linux (Master)..."
                        sh "python3 salary_calc.py \"${params.EMPLOYEE_NAME}\" ${params.HOURLY_RATE} ${params.HOURS_WORKED} ${params.IS_HOLIDAY} \"${params.DATE}\""
                    } else {
                        echo "Running on Windows (Agent)..."
                        // בווינדוס משתמשים ב-bat ובדרך כלל רק ב-python (בלי 3)
                        bat "python salary_calc.py \"${params.EMPLOYEE_NAME}\" ${params.HOURLY_RATE} ${params.HOURS_WORKED} ${params.IS_HOLIDAY} \"${params.DATE}\""
                    }
                }
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
                reportFiles: 'salary_slip.html',
                reportName: 'Salary Report',
                reportTitles: 'Salary Slip'
            ])
        }
    }
}
