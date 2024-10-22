pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    } 
    stages {
         stage('Build') { 
            steps {
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
         stage('Install Dependencies') {
            steps {
                sh 'pip install pytest'
            }
        }
        stage('Test') {
            steps {
//Add pytest to PATH and run tests
                sh 'export PATH=$PATH:/var/lib/jenkins/.local/bin && pytest --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
         stage('Deliver') {
            steps {
                sh "pyinstaller --onefile sources/add2vals.py"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
