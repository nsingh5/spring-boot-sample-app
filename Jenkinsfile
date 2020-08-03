pipeline {
    agent  any

    stages {
        stage ('Initialize') {
            steps {
                script {
                def mvnHome = tool 'Maven 3.3'
               echo "initailting"
                echo "${mvnHome}"
                }}
        }

        stage ('Build') {
            steps {
                script {
                def mvnHome = tool 'Maven 3.3'
             // bat "mvn install -Dmaven.test.skip=true"
               //fortifyClean addJVMOptions: '', buildID: 'test', logFile: '', maxHeap: ''
            }
            }
        }
         stage ('unit test') {
            steps {
                script {
                def mvnHome = tool 'Maven 3.3'
             // bat "mvn test"
               //fortifyClean addJVMOptions: '', buildID: 'test', logFile: '', maxHeap: ''
                }}
           
        }
        stage('Sonar scan execution') {
            // Run the sonar scan
            steps {
                script {  
                def mvnHome = tool 'Maven 3.3'
                        withSonarQubeEnv {
                            
                    try {  
                             bat "mvn  verify sonar:sonar -Dsonar.host.url=http://localhost:9007/ -Dmaven.test.failure.ignore=true"            
                        } catch (err) {
                            echo err.getMessage()
                        }
                 }
                }
            }
        }
                // waiting for sonar results based into the configured web hook in Sonar server which push the status back to jenkins
        stage('Sonar scan result check') {
            steps {
                
                    timeout(time: 2, unit: 'MINUTES') {
                        retry(3) {
                            script {
                                 try {  
                                def qg = waitForQualityGate()
                                if (qg.status != 'OK') {
                                    echo "Pipeline aborted due to quality gate failure: ${qg.status}"
                                }
                                         } catch (err) {
                                        echo err.getMessage()
                                     }
                            }
                        }
                        }
            }
        }
        
        stage('fortify scan execution') {
            // Run the sonar scan
            steps {
                script {
                       try {  
                           fortifyScan addJVMOptions: '', addOptions: '', buildID: 'test', customRulepacks: '', logFile: '', maxHeap: '', resultsFile: 'testresult'
                        } catch (err) {
                            echo err.getMessage()
                        }
                   
                }
            }
        }
    }
}
