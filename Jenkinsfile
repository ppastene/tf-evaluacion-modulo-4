pipeline {
    agent any

    stages {
        stage('Clonar ppastene/td-selenium-test') {
            steps {
                script {
                    try {
                        sh 'git clone https://github.com/ppastene/td-selenium-test.git'
                    } catch (Exception e) {
                        echo "El repositorio ya existe, omitiendo la clonación."
                    }
                }
            }
        }
        stage('Ejecutar Selenium Tests') {
            steps {
                dir('td-selenium-test') {
                    sh 'mvn clean test'
                }
            }
        }
        stage('Clonar ppastene/td-unit-tests') {
            steps {
                script {
                    try {
                        sh 'git clone https://github.com/ppastene/td-unit-tests.git'
                    } catch (Exception e) {
                        echo "El repositorio ya existe, omitiendo la clonación."
                    }
                }
            }
        }
        stage('Ejecutar JUnit') {
            steps {
                dir('td-unit-tests') {
                    sh 'mvn clean test'
                }
            }
        }
        stage('Verificar archivos en shared') {
            steps {
                sh 'ls -la /shared'
            }
        }
        stage('Ejecutar JMeter') {
            steps {
                script {
                    sh '/opt/jmeter/bin/jmeter -n -t /shared/clase4_m4.jmx -l /shared/jmeter-reports/report.jtl'
                }
            }
        }
        stage('Ejecutar SOAPUI') {
            steps {
                script {
                    sh  '/opt/soapui/bin/testrunner.sh -r -j -f "soapui-report.xml" -d "/shared/soapui-reports/" /shared/soapui-evaluacion-final.xml'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/shared/junit-reports/*.xml', allowEmptyArchive: true
            archiveArtifacts artifacts: '**/shared/jmeter-reports/report.jtl', allowEmptyArchive: true
            archiveArtifacts artifacts: '**/shared/soapui-reports/*.xml', allowEmptyArchive: true
        }
    }
}
