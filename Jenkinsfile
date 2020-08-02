pipeline {
    agent any

    stages {
        stage ('Initialize') {
            steps {
               echo "initailting"
            }
        }

        stage ('Build') {
            steps {
               bat "dir"
              bat "mvn install -DskipTests"
               //fortifyClean addJVMOptions: '', buildID: 'test', logFile: '', maxHeap: ''
            }
            post {
                success {
                   echo "passed"
                }
            }
        }
    }
}
