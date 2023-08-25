properties([
    pipelineTriggers([pollSCM('H/2 * * * *')])
])

node {
    // Defining the docker agent 
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        
        // Checkout the code from SCM
        checkout scm

        // Build Stage
        stage('Build') {
            sh 'mvn -B -DskipTests clean package'
        }

        // Test Stage
        stage('Test') {
            try {
                sh 'mvn test'
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
        }

        // Deliver Stage
        stage('Deliver') {
            sh './jenkins/scripts/deliver.sh'
        }
    }
}