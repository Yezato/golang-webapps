pipeline {
        agent any

        tools {
            go 'go1.16'
        }

        environment{
            //DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred')
            DOCKER_REPO = 'FILE_DOCKER'
            doError = '0'
            GIT_REPO_NAME = env.GIT_URL.replaceFirst(/^.*\/([^\/]+?).git$/, '$1')
        }
            options {
                buildDiscarder(logRotator(numToKeepStr: '20')) 
        }  

        stages {        
            stage('Pre Test') {
                when {
                    branch 'main'
                }
                steps {
                    echo 'Installing dependencies'
                    sh 'go version'
                    sh 'go get -u github.com/golang-jwt/jwt/v4'
                }
            }
            stage('Build Artifact') {
                when {
                    branch 'main'
                }
                steps {
                    echo 'Compiling and building'
                    sh 'go mod tidy'
                    sh 'go build'
                }
            }
            stage('Test') {
                when {
                    branch 'main'
                }
                steps {
                    echo 'Testing'
                    echo 'Running vetting'
                    sh 'go vet .'
                    echo 'Running linting'
                    sh 'golint .'
                    echo 'Running test'
                    //sh 'go test ./...  -coverpkg=./... -coverprofile ./coverage.out'
                    //sh 'go tool cover -func ./coverage.out'          
                }
            }
            
            
            stage('Build Docker Image') {
                when {
                    branch 'development'
                }

                steps {
                    echo 'Build web Apps on Progress...'
                    //build Golang into Executable
                    //Dockerize Golang
                    sh 'docker build . -t ${DOCKER_REPO}:${GIT_REPO_NAME}-${BUILD_NUMBER}-SNAPSHOT'
                }
            }
           
        }
    }
