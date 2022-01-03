#!/usr/bin/env groovy

/* `buildPlugin` step provided by: https://github.com/jenkins-infra/pipeline-library */
//buildPlugin()

pipeline {
    agent {
        label 'Docker 04'
    }
    options {
        ansiColor('xterm')
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
        skipDefaultCheckout()
        timeout(activity: true, time: 20, unit: 'MINUTES')
        timestamps()
    }
    stages {
        stage('Prepare workspace') {
            environment {
                HTTP_PROXY = 'proxy.dev.ttw:3128'
                HTTPS_PROXY = 'proxy.dev.ttw:3128'
            }
            steps {
                plainCheckoutFromGit(scm, env, [[$class: 'CloneOption', noTags: false], [$class: 'LocalBranch']])
                configFileProvider([configFile(fileId: '1d2a39a0-89eb-4bb3-ac3a-7cac7bb610eb', targetLocation: 'mavenSettings.xml')])
            }
        }
        stage('Build package') {
            steps {
				script {
                    docker.image('nexus.ttt-sp.com:16000/maven:3-openjdk-8-slim').inside {
                        sh 'mvn -Dmaven.repo.local=/usr/src/mavenbuild/.m2 -settings mavenSettings.xml clean package'
                    }
                }
            }
        }
    }
}
