#!/usr/bin/groovy

@Library(['github.com/indigo-dc/jenkins-pipeline-library']) _

pipeline {
    agent {
        label 'docker-build'
    }

    environment {
        dockerhub_repo = "deephdc/deep-oc-phytoplankton-classification"
    }

    stages {
        stage('DockerHub delivery') {
            when {
                anyOf {
                    branch 'master'
                    branch 'test'
                    buildingTag()
                }
            }
            steps{
                checkout scm
                script {
                    image_id = DockerBuild(dockerhub_repo, env.BRANCH_NAME)
                }
            }
            post {
                success {
                    DockerPush(image_id)
                }
                failure {
                    DockerClean()
                }
                always {
                    cleanWs()
                }
            }
        }

        stage("Render metadata on the marketplace") {
            when {
                allOf {
                    branch 'master'
                    changeset 'metadata.json'
                }
            }
            steps {
                script {
                    def job_result = JenkinsBuildJob("Pipeline-as-code/deephdc.github.io/pelican")
                    job_result_url = job_result.absoluteUrl
                }
            }
        }
    }
}
