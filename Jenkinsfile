pipeline {
    agent any

    environment {
        // ضع اسم المستخدم الخاص بك على Docker Hub هنا
        DOCKERHUB_USERNAME = 'haddad2003'
        // اسم الصورة (Repository)
        APP_NAME = 'hello-spring'
        // التاج (Tag)
        IMAGE_TAG = 'latest'
        // اسم الحاوية عند التشغيل
        CONTAINER_NAME = 'hello-app'

        // يجب أن يكون هذا الـ ID مطابقاً لما أنشأته في Jenkins Credentials
        DOCKER_CRED_ID = 'dockerhub'
    }

    stages {
        stage('Build JAR') {
            steps {
                echo '🔨 Compilation de l\'application...'
                // استخدام mvnw.cmd الخاص بويندوز
                bat 'mvnw.cmd clean package -DskipTests'
            }
        }

        stage('Build & Push Docker') {
            steps {
                echo '🐳 Construction et publication de l\'image Docker...'
                script {
                    // نستخدم withCredentials لجلب كلمة المرور بأمان بدلاً من docker.withRegistry
                    withCredentials([usernamePassword(credentialsId: DOCKER_CRED_ID, usernameVariable: 'USER', passwordVariable: 'PASS')]) {

                        // 1. تسجيل الدخول
                        bat 'docker login -u %USER% -p %PASS%'

                        // 2. بناء الصورة (Build)
                        // نستخدم %USER% للدلالة على اسم المستخدم من Credentials
                        bat "docker build -t %USER%/%APP_NAME%:%IMAGE_TAG% ."

                        // 3. رفع الصورة (Push)
                        bat "docker push %USER%/%APP_NAME%:%IMAGE_TAG%"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Déploiement de l\'application...'
                // أوامر التشغيل (تم استخدام exit 0 لتجاهل الأخطاء إذا كانت الحاوية غير موجودة)
                bat """
                    docker stop ${CONTAINER_NAME} || exit 0
                    docker rm ${CONTAINER_NAME} || exit 0
                    docker pull ${DOCKERHUB_USERNAME}/${APP_NAME}:${IMAGE_TAG}
                    docker run -d --name ${CONTAINER_NAME} -p 8080:8080 ${DOCKERHUB_USERNAME}/${APP_NAME}:${IMAGE_TAG}
                """
            }
        }
    }

    post {
        always {
            // تسجيل الخروج لأمان أكثر
            bat 'docker logout'
        }
        success {
            echo '✅ Pipeline exécuté avec succès!'
        }
        failure {
            echo '❌ Le pipeline a échoué.'
        }
    }
}