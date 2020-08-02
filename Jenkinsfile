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
              bat "mvn build -Dmaven.test.skip=true"
               //fortifyClean addJVMOptions: '', buildID: 'test', logFile: '', maxHeap: ''
            }
           
        }
         stage ('unit test') {
            steps {
               bat "dir"
              bat "mvn test"
               //fortifyClean addJVMOptions: '', buildID: 'test', logFile: '', maxHeap: ''
            }
           
        }
    }
}
