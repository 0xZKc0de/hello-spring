pipeline {
    agent any

    environment {
        // تأكد من وضع اسم المستخدم الصحيح هنا
        DOCKERHUB_REPO = 'VOTRE-USERNAME/hello-spring'
        CONTAINER_NAME = 'hello-app'
    }

    stages {
        stage('Build JAR') {
            steps {
                echo '🔨Compilation de l application...'
                // تم تغيير sh إلى bat
                // وتم استخدام mvnw.cmd الخاص بويندوز بدلاً من mvn فقط
                bat 'mvnw.cmd clean package -DskipTests'
            }
        }

        stage('Build & Push Docker') {
            steps {
                echo '🐳Construction et publication de l image Docker...'
                script {
                    docker.withRegistry('', 'dockerhub') {
                        def app = docker.build("${DOCKERHUB_REPO}:latest")
                        app.push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀Déploiement de l application...'
                // تم تغيير sh إلى bat
                bat """
                    docker stop ${CONTAINER_NAME} || exit 0
                    docker rm ${CONTAINER_NAME} || exit 0
                    docker pull ${DOCKERHUB_REPO}:latest
                    docker run -d --name ${CONTAINER_NAME} -p 8080:8080 ${DOCKERHUB_REPO}:latest
                """
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline exécuté avec succès!'
        }
        failure {
            echo '❌ Le pipeline a échoué.'
        }
    }
}