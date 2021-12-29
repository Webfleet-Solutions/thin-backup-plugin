#!/usr/bin/env groovy

/* `buildPlugin` step provided by: https://github.com/jenkins-infra/pipeline-library */
//buildPlugin()

pipeline {
    agent {
        label 'docker'
    }
    options {
        ansiColor('xterm')
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
        skipDefaultCheckout()
        timeout(activity: true, time: 20, unit: 'MINUTES')
        timestamps()
/*        office365ConnectorWebhooks([[
            name: 'Sky Notifications',
            notifyBackToNormal: true,
            notifyFailure: true,
            notifyRepeatedFailure: true,
            url: 'https://webfleet.webhook.office.com/webhookb2/809b395f-fac9-4841-89ed-04da45c71f1d@e648a634-1151-497c-9797-0f975bddecc0/IncomingWebhook/8d9c1963b77049ef94363a683d53d403/37ff97a0-7b51-4448-9516-722f9e10fdda'
        ]]) */
    }
    stages {
        stage('Prepare workspace') {
            steps {
                plainCheckoutFromGit(scm, env, [[$class: 'CloneOption', noTags: false], [$class: 'LocalBranch']])
                configFileProvider([configFile(fileId: '1d2a39a0-89eb-4bb3-ac3a-7cac7bb610eb', targetLocation: 'mavenSettings.xml')])
            }
        }
        stage('Build package') {
            steps {
                docker.image('nexus.ttt-sp.com:16000/maven:3-openjdk-8-slim').inside {
                    sh 'mvn -Dmaven.repo.local=/usr/src/mavenbuild/.m2 -settings mavenSettings.xml clean package'
                }
            }
        }
    }
}
