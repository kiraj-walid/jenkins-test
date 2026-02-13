pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Code récupéré depuis GitHub'
                git branch: 'main', url: 'https://github.com/kiraj-walid/jenkins-test.git'
            }
        }
        
        stage('Build Maven') {
            steps {
                echo 'Compilation des microservices...'
                sh '''
                    cd ms-discovery && mvn clean package -DskipTests
                    cd ../ms-config && mvn clean package -DskipTests
                    cd ../ms-livre && mvn clean package -DskipTests
                    cd ../ms-etudiant && mvn clean package -DskipTests
                    cd ../ms-emprunt && mvn clean package -DskipTests
                    cd ../ms-gateway && mvn clean package -DskipTests
                '''
            }
        }
        
        stage('Build Docker Images') {
            steps {
                echo 'Construction des images Docker...'
                sh 'docker-compose build'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Déploiement de l\'application...'
                sh '''
                    docker-compose down
                    docker-compose up -d
                '''
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline exécuté avec succès!'
        }
        failure {
            echo '❌ Le pipeline a échoué'
        }
    }
}