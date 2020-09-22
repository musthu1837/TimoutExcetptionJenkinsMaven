pipeline {
    agent any

    stages {
        stage("build"){
            sh "mvn clean package"
        }
        stage("dev") {
            options {
                timeout(time: 3, unit: "SECONDS")
            }

            steps {
                script {
                    Exception caughtException = null

                    catchError(buildResult: 'SUCCESS', stageResult: 'ABORTED') { 
                        try { 
                            echo "Started stage A"
                            sleep(time: 5, unit: "SECONDS")
                        } catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
                            error "Caught ${e.toString()}" 
                        } catch (Throwable e) {
                            caughtException = e
                        }
                    }

                    if (caughtException) {
                        error caughtException.message
                    }
                }
            }
        }

        stage("prod") {
            steps {
                echo "Started stage prod"
            }
        }
    }
}
