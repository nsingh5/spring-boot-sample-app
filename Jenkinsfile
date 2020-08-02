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
              bat "mvn install -Dmaven.test.skip=true"
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
        stage('Sonar scan execution') {
            // Run the sonar scan
            steps {
                script {
                    withSonarQubeEnv {

                        bat "mvn  verify sonar:sonar -Dintegration-tests.skip=true -Dmaven.test.failure.ignore=true"
                    }
                }
            }
        }
    }
}
