pipeline {
    agent any
    environment {
        NEXUS_USER         = credentials('nexus-user')
        NEXUS_PASSWORD     = credentials('nexus-pass')
    }
     parameters {
        choice(
            name:'compileTool',
            choices: ['Maven', 'Gradle'],
            description: 'Seleccione herramienta de compilacion'
        )
    }
    stages {
        stage("Pipeline"){
            steps {
                script{
                  switch(params.compileTool)
                    {
                        case 'Maven':
                            def ejecucion = load 'maven.groovy'
                            ejecucion.call()
                        break;
                        case 'Gradle':
                            def ejecucion = load 'gradle.groovy'
                            ejecucion.call()
                        break;
                    }
                }
            }
            post{
                success{
                    slackSend color: 'good', message: "[Erickson Bravo] [${JOB_NAME}] [${BUILD_TAG}] Ejecucion Exitosa", teamDomain: 'dipdevopsusac-tr94431', tokenCredentialId: 'token-slack'
                }
                failure{
                    slackSend color: 'danger', message: "[Erickson Bravo] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'dipdevopsusac-tr94431', tokenCredentialId: 'token-slack'
                }
            }
        }
    }
}