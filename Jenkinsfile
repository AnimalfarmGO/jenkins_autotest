pipeline {
    agent any
    stages {
        stage('run docker nginx') {
            steps {
                sh 'docker run -it -d -p 9889:80 --name web -v /tmp/site-content:/usr/share/nginx/html nginx'
            }
        }
        stage('Checking response code') {
            steps {
                sh '''
                   response=$(curl --write-out '%{http_code}' --silent --output /dev/null http://localhost:9889/)
                   [[ $response == 200 ]] && echo 'page is available'
                    
                '''
                    }
        }
        stage('curl test') {
            steps {
                sh '''
                    curl http://localhost:9889 > index.txt
                    md5sum index.txt site-content/index.html > hashes.txt
                    md5sum --check hashes.txt
                    ls 
                    ls site-content
                    curl -o /dev/null -s -w "%{http_code}" http://localhost:9889
                   ''' 
            }
        } 
                
        stage('Del') {
            steps {
                sh 'docker rm -f web'
            }
        }
    }
}
