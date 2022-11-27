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
                    cd /tmp/hash
                    curl http://localhost:9889 > index.txt
                    md5sum index.txt /tmp/site-content/index.html > /tmp/hash/hashes.txt
                    md5sum --check hashes.txt
                    ls 
                    cat hashes.txt
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
