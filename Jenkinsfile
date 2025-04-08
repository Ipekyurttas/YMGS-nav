pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        IMAGE_NAME = "ymg-ders-test"
        CONTAINER_NAME = "nginx-container"
    }

    stages {
        stage("Repo Klonlama") {
            steps {
                git url: "https://github.com/Ipekyurttas/YMGS-nav.git", branch: "main"
            }
        }

        stage("Docker image Oluştur") {
            steps {
                echo "Docker image oluşturuluyor"
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage("Eski container durdur") {
            steps {
                echo "Eski container durduruluyor (varsa)"
                bat "docker rm -f %CONTAINER_NAME% || exit 0"
            }
        }

        stage("Yeni container oluştur") {
            steps {
                echo "Yeni container oluşturuluyor"
                bat "docker run -d --name %CONTAINER_NAME% -p 4444:80 %IMAGE_NAME%"
            }
        }
    }

    post {
        success {
            echo "Yayın başarılı: http://localhost:4444"
        }
        failure {
            echo "Pipeline başarısız oldu"
        }
    }
}
