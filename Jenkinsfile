pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('Clone') {
            steps {
                echo "📥 Clonage du projet depuis GitHub"
                git 'https://github.com/Mohamedchaouachi/CRUDEtudiant_Using_SPRINGFramework.git'
            }
        }

        stage('Build') {
            steps {
                echo "🔨 Compilation et packaging avec Maven (tests skipped)"
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Construction de l’image Docker"
                sh 'docker build -t crudetudiant:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "🚀 Lancement du conteneur Docker"
                sh '''
                    docker stop crudetudiant-container || true
                    docker rm crudetudiant-container || true
                    docker run -d --name crudetudiant-container -p 8080:8080 crudetudiant:latest
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "🕵️ Lancement de l’analyse SonarQube"
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://10.255.255.254:9000'
                }
            }
        }

    }

    post {
        success {
            echo "✅ Pipeline terminé avec succès"
        }
        failure {
            echo "❌ Pipeline échoué, vérifier les logs"
        }
    }
}
