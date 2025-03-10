node {
    checkout scm
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            catch(Exception e) {
                
            }
            finally {
                junit 'test-reports/results.xml'
            }
            
        }
    }
    stage ("Manual Approval") {
        input 'Lanjutkan ke tahap Deploy?'
    }
    stage ("Deploy") {
        sleep 60
    }
}
