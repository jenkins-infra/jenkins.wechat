pipeline {
    agent {
        label 'linux'
    }

    options {
        disableConcurrentBuilds()
        quietPeriod 30
    }

    stages{
        stage("Notify"){
            steps{
                sh 'curl http://baidu.com'
            }
        }
    }
}
