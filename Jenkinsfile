pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Pulling...' + env.BRANCH_NAME
                    def mvnHome = tool 'Maven 3.3'
                    if (isUnix()) {
                        sh "'${mvnHome}/bin/mvn' -Dmaven.test.skip=true clean install"

                    } else {
                        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.skip=true clean install/)
                    }
                }
            }
        }

        stage('unit test') {
            steps {
                script {
                    def mvnHome = tool 'Maven 3.3'
                    if (isUnix()) {
                        sh "'${mvnHome}/bin/mvn'  test"
                    } else {
                        bat(/"${mvnHome}\bin\mvn" test/)
                    }

                }
            }
        }

        stage('Sonar scan execution') {
            steps {
                script {
                    def mvnHome = tool 'Maven 3.3'
                    withSonarQubeEnv {

                        try {
                            if (isUnix()) {
                                sh "'${mvnHome}/bin/mvn'  sonar:sonar  -Dsonar.projectKey=DGFChatbot  -Dsonar.host.url=https://sonarqube.dhl.com  -Dsonar.login=4d77e8116d228811203c828c59ee74cffedad3b8 -Dmaven.test.failure.ignore=true"
                            } else {
                                bat(/"${mvnHome}\bin\mvn"  sonar:sonar  -Dsonar.projectKey=DGFChatbot  -Dsonar.host.url="https://sonarqube.dhl.com" -Dsonar.login=4d77e8116d228811203c828c59ee74cffedad3b8 -Dmaven.test.failure.ignore=true/)
                            }

                        } catch (err) {
                            echo err.getMessage()
                        }
                    }
                }
            }
        }
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
