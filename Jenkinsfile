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
        docker.image('cdrx/pyinstaller-linux:python2').inside('-u 0:0 --entrypoint="" -e PATH="/root/.pyenv/shims:/root/.pyenv/bin:$PATH"') {
            try {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            catch(Exception e) {

            }
        }
        withCredentials([sshUserPrivateKey(credentialsId: 'c86ddcb3-cc94-4b96-9c71-88f5cb257d10', keyFileVariable: 'SSH_KEY')]) {
            sh '''
            scp -i $SSH_KEY dist/add2vals beluvedia@74.48.19.195:/opt/submission/
            scp -i $SSH_KEY beluvedia@74.48.19.195 "docker build -t add2vals:latest -t add2vals:$BUILD_NUMBER /opt/submission && docker run --rm add2vals 3 5"
            '''
        }
        sleep 60
    }
}
