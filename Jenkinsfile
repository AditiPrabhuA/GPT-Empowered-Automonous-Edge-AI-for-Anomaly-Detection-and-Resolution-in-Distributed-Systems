pipeline {
    agent any

    stages {
        stage('Prepare Client') {
            steps {
                sh 'ssh aditi@10.20.202.175 "cd /Users/aditi/Downloads/DDOS && make clean"'
            }
        }
        stage('Capture and Process Traffic') {
            steps {
                sh 'ssh aditi@10.20.202.175 "cd /Users/aditi/Downloads/DDOS && make capture_and_process"'
            }
        }
        stage('Start MQTT Broker on Server') {
            steps {
                sh 'mosquitto -d'
            }
        }
        stage('Start Subscriber on Server') {
            steps {
                sh 'python3 subscriber.py &'
            }
        }
        stage('Publish Data from Client') {
            steps {
                sh 'ssh aditi@10.20.202.175 "cd /Users/aditi/Downloads/DDOS && python3 publisher.py"'
            }
        }
        stage('Run Model on Server') {
            steps {
                sh 'python3 run_model.py'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'pkill -f subscriber.py'
                sh 'pkill mosquitto'
            }
        }
    }
}
