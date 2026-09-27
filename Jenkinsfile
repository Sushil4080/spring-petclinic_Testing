pipeline {

    agent any

    environment {
        SONAR_PROJECT_KEY  = 'petclinic'
        SONAR_PROJECT_NAME = 'petclinic'

        IMAGE_NAME = 'spring-petclinic'
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        // ============================================================
        // 1. CHECKOUT
        // ============================================================
        stage('Checkout') {
            steps {
                echo '===== Checkout ====='

                git url: 'https://github.com/Sushil4080/spring-petclinic_Testing.git',
                    branch: 'main'
            }
        }

        // ============================================================
        // 2. BUILD APPLICATION
        // ============================================================
        stage('Build Application') {
            steps {
                echo '===== Build Application ====='

                sh '''
                    chmod +x mvnw
                    ./mvnw clean package -DskipTests
                '''
            }
        }

        // ============================================================
        // 3. UNIT TEST
        // ============================================================
        stage('Unit Test') {
            steps {
                echo '===== Unit Test ====='

                sh '''
                    ./mvnw test
                '''
            }

            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        // ============================================================
        // 4. SONARQUBE ANALYSIS
        // ============================================================
        stage('SonarQube Analysis') {
            steps {
                echo '===== SonarQube Analysis ====='

                withSonarQubeEnv('SonarQube') {

                    script {

                        def scannerHome = tool 'SonarScanner'

                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                              -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                              -Dsonar.projectName=${SONAR_PROJECT_NAME} \
                              -Dsonar.sources=src/main/java \
                              -Dsonar.tests=src/test/java \
                              -Dsonar.java.binaries=target/classes
                        """
                    }
                }
            }
        }

        // ============================================================
        // 5. TRIVY FILESYSTEM SCAN
        // ============================================================
        stage('Trivy Filesystem Scan') {
            steps {
                echo '===== Trivy Filesystem Scan ====='

                sh '''
                    trivy fs \
                      --severity HIGH,CRITICAL \
                      --exit-code 0 \
                      --no-progress \
                      .
                '''
            }
        }

        // ============================================================
        // 6. DOCKER BUILD
        // ============================================================
        stage('Docker Build') {
            steps {
                echo '===== Docker Build ====='

                sh """
                    docker build \
                      -t ${IMAGE_NAME}:${IMAGE_TAG} \
                      -t ${IMAGE_NAME}:latest \
                      .
                """
            }
        }

        // ============================================================
        // 7. TRIVY DOCKER IMAGE SCAN
        // ============================================================
        stage('Trivy Docker Image Scan') {
            steps {
                echo '===== Trivy Docker Image Scan ====='

                sh """
                    trivy image \
                      --severity HIGH,CRITICAL \
                      --exit-code 0 \
                      --no-progress \
                      ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        // ============================================================
        // 8. DOCKER IMAGES
        // ============================================================
        stage('Docker Images') {
            steps {
                echo '===== Docker Images ====='

                sh '''
                    docker images | grep spring-petclinic
                '''
            }
        }

        // ============================================================
        // 9. DOCKER RUN
        // ============================================================
        stage('Docker Run') {
            steps {
                echo '===== Docker Run ====='

                sh '''
                    docker rm -f spring-petclinic-test 2>/dev/null || true

                    docker run -d \
                      --name spring-petclinic-test \
                      -p 8081:8080 \
                      spring-petclinic:latest

                    echo "Waiting for application..."
                    sleep 15

                    docker ps | grep spring-petclinic-test
                '''
            }
        }

        // ============================================================
        // 10. CONTAINER VERIFICATION
        // ============================================================
        stage('Container Verification') {
            steps {
                echo '===== Container Verification ====='

                sh '''
                    echo "Testing application..."

                    curl -I http://localhost:8081 || true

                    echo "Container logs:"

                    docker logs --tail 30 spring-petclinic-test
                '''
            }
        }

        // ============================================================
        // 11. CLEANUP
        // ============================================================
        stage('Cleanup') {
            steps {
                echo '===== Cleanup ====='

                sh '''
                    docker rm -f spring-petclinic-test 2>/dev/null || true
                '''
            }
        }
    }

    // ================================================================
    // POST ACTIONS
    // ================================================================
    post {

        success {
            echo '======================================'
            echo ' DevSecOps Pipeline SUCCESS'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo ' Pipeline FAILED'
            echo ' Check the failed stage'
            echo '======================================'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
