pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
       
        stage('Run Security Scans') {
            steps {
                sh '''
        mkdir -p $PWD/reports $PWD/artifacts;
        docker run \
            -v $PWD/reports:/arachni/reports ahannigan/docker-arachni \
            bin/arachni http://staging.example.io --report-save-path=reports/example.io.afr;
        docker run --name=arachni_report  \
            -v $PWD/reports:/arachni/reports ahannigan/docker-arachni \
            bin/arachni_reporter reports/example.io.afr --reporter=html:outfile=reports/example-io-report.html.zip;
        docker cp arachni_report:/arachni/reports/example-io-report.html.zip $PWD/artifacts;
        docker rm arachni_report;
    '''
        archiveArtifacts artifacts: 'artifacts/**', fingerprint: true

            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
