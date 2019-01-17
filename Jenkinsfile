pipeline {
    agent any

     triggers {
        pollSCM('*/5 * * * 1-5')
    }
    options {
        skipDefaultCheckout(true)
        // Keep the 10 most recent builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }
    enviroment {
        WORKON_HOME=$HOME/.virtualenvs
        PROJECT_HOME=$HOME/Devel
        PATH = /usr/local/lib/python3.7/site-packages:$PATH
    }
    stages {

        stage ("Code pull"){
            steps{
                checkout scm
            }
        }
        stage('Build environment') {
            steps {
                sh '''mkvirtualenv ${BUILD_TAG}
                      workon ${BUILD_TAG} 
                      pip3 install -r requirements.txt
                    '''
            }
        }
        stage('Test environment') {
            steps {
                sh '''source activate ${BUILD_TAG} 
                      pip3 list
                      which pip3
                      which python3
                    '''
            }
        }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}