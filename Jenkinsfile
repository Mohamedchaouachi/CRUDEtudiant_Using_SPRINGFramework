pipeline {
    agent any

    tools {
        maven 'Maven'  // Nom de l’installation Maven dans Jenkins
    }

    environment {
        // Définir éventuellement JAVA_HOME si nécessaire
        JAVA_HOME = tool name: 'JDK 17', type: 'jdk'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
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

        stage('SonarQube Analysis') {
            steps {
                echo "🕵️ Lancement de l’analyse SonarQube"
                // 'SonarQube' = nom configuré dans Jenkins → Manage Jenkins → Configure System → SonarQube Servers
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000'
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
