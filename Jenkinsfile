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
    stage('Deliver') {
        docker.image('cdrx/pyinstaller-linux:python2').inside('-u 0:0 --entrypoint="" -e PATH="/root/.pyenv/shims:/root/.pyenv/bin:$PATH"') {
            try {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            catch(Exception e) {

            }
            archiveArtifacts 'dist/add2vals'
        }
    }
}