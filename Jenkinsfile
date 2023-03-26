#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('build app') {
            steps {
                script {
                    echo "building the application..."
                    sh 'mvn clean package'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t waseemkhandocker/my-docker-repo:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push waseemkhandocker/my-docker-repo:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                }
            }
        }
        stage('commit version update') {
            steps {
              //  script {
              //      withCredentials([usernamePassword(credentialsId: 'Git-Lab_Id', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        // git config here for the first time run
                       // sh 'git config --global user.email "jenkins@example.com"'
                       // sh 'git config --global user.name "jenkins"'

                       // sh "git remote set-url origin https://${USER}:${PASS}@gitlab.com/waseem.khan4034/java-maven-app.git"              
                      //  sh 'git add .'
                      //  sh 'git commit -m "ci: version bump"'
                      //  sh 'git push origin jenkins-jobs'
                  //  }
                }
            }
        }
    }
}

