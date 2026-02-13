pipeline {
    agent any
    
    environment {
        DOCKER_COMPOSE_VERSION = '2.0'
        MAVEN_OPTS = '-Dmaven.repo.local=.m2/repository'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Code récupéré depuis GitHub'
                git branch: 'main', 
                    url: 'https://github.com/kiraj-walid/jenkins-test.git'
            }
        }
        
        stage('Build Maven') {
            parallel {
                stage('Discovery Service') {
                    steps {
                        dir('ms-discovery') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                }
                stage('Config Service') {
                    steps {
                        dir('ms-config') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                }
                stage('Livre Service') {
                    steps {
                        dir('ms-livre') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                }
                stage('Etudiant Service') {
                    steps {
                        dir('ms-etudiant') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                }
                stage('Emprunt Service') {
                    steps {
                        dir('ms-emprunt') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                }
                stage('Gateway Service') {
                    steps {
                        dir('ms-gateway') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                }
            }
        }
        
        // stage('Run Tests') {
        //     steps {
        //         echo 'Exécution des tests unitaires...'
        //         sh '''
        //             cd ms-discovery && mvn test || true
        //             cd ../ms-config && mvn test || true
        //             cd ../ms-livre && mvn test || true
        //             cd ../ms-etudiant && mvn test || true
        //             cd ../ms-emprunt && mvn test || true
        //             cd ../ms-gateway && mvn test || true
        //         '''
        //     }
        // }
        
        // stage('Build Docker Images') {
        //     steps {
        //         echo 'Construction des images Docker...'
        //         sh 'docker compose build --no-cache'
        //     }
        // }
        
        // stage('Stop Old Containers') {
        //     steps {
        //         echo 'Arrêt des anciens containers...'
        //         sh 'docker compose down || true'
        //     }
        // }
        
        stage('Deploy') {
            steps {
                echo 'Déploiement de l\'application...'
                sh 'docker compose up -d'
                
                echo 'Vérification du déploiement...'
                sh 'docker compose ps'
            }
        }
        
        stage('Health Check') {
            steps {
                echo 'Vérification de la santé des services...'
                script {
                    sleep(time: 30, unit: 'SECONDS')
                    sh 'docker compose ps | grep "Up" || exit 1'
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline exécuté avec succès!'
            // Ajoutez ici vos notifications (email, Slack, etc.)
        }
        failure {
            echo '❌ Le pipeline a échoué'
            sh 'docker compose logs'
            // Ajoutez ici vos notifications d'échec
        }
        always {
            echo 'Nettoyage des images Docker inutilisées...'
            sh 'docker image prune -f || true'
        }
    }
}