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

        stage('Approval') {
            input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
        }

        // Deliver Stage
        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
        }

        // Sleep for 1 minute
        stage('Pause') {
            sleep time: 1, unit: 'MINUTES'
        }

        // Completion Stage
        stage('Completion') {
            echo 'Eksekusi pipeline sukses!'
        }
    }
}