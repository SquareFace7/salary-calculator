pipeline {
    agent any 
    
    parameters {
        // בחירת הסוכן עליו ירוץ הג'וב (דרישה 4.2 במטלה)
        choice(name: 'TargetNode', choices: ['built-in', 'agent1'], description: 'Select the node to run the script on')
        
        // פרמטרים לסקריפט השכר
        string(name: 'EMPLOYEE_NAME', defaultValue: 'Israel Israeli', description: 'Enter Employee Name')
        string(name: 'HOURLY_RATE', defaultValue: '50', description: 'Enter Hourly Rate')
        string(name: 'HOURS_WORKED', defaultValue: '160', description: 'Enter Total Hours')
        booleanParam(name: 'IS_HOLIDAY', defaultValue: false, description: 'Did the employee work on a holiday?')
        string(name: 'DATE', defaultValue: '2023-10-01', description: 'Enter Date (YYYY-MM-DD)')
    }

    stages {
        stage('Checkout') {
            steps {
                // שלב 1: הורדת הקוד מגיטהאב
                checkout scm
                echo 'Code checked out successfully.'
            }
        }
        
        stage('Validate Parameters') {
            steps {
                script {
                    // שלב 2: בדיקה פשוטה שהערכים לא ריקים (לוגיקה נוספת יש בסקריפט פייתון)
                    if (params.EMPLOYEE_NAME == '') {
                        error "Employee Name cannot be empty!"
                    }
                    echo "Validating inputs for user: ${params.EMPLOYEE_NAME}"
                }
            }
        }

        stage('Run Script') {
            agent { label "${params.TargetNode}" } // הרצה על הסוכן שנבחר
            steps {
                // שלב 3: הרצת הסקריפט (שים לב: אם אתה בווינדוס שנה את sh ל-bat)
                echo 'Running salary calculation script...'
                sh "python3 salary_calc.py \"${params.EMPLOYEE_NAME}\" ${params.HOURLY_RATE} ${params.HOURS_WORKED} ${params.IS_HOLIDAY} \"${params.DATE}\""
            }
        }
    }

    post {
        always {
            // שלב 4: שמירת הדוח והלוגים כדי שיופיעו בג'נקינס
            archiveArtifacts artifacts: '*.html, *.log', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline finished successfully! HTML report generated.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
