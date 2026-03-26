// pipeline {
//      agent 
//     {
//         // Environnrement Node, npm, navigateur chromium, git

//         docker {
//             image  'cypress/included:cypress-15.11.0-node-24.14.0-chrome-145.0.7632.116-1-ff-148.0-edge-145.0.3800.70-1'
//             args '--user=root --entrypoint=""'
//         }

//     }

//     // paramètre pour ajouter les tags et rapport
//     // tags
//     // rapport
//     parameters{
//         choice(name:"tag",choices:["@smoke","@e2e"],description:"choisissez le tag")
//         choice(name:"browser",choices:["chrome","firefox","edge"],description:"choisissez le navigateur")
//         booleanParam(name:"run_with_tag",defaultValue:false,description:"lancer avec ou sans le tag")
//     }
//     // browser if browser == chromium on lance les testes sinon on affiche aucun test est lancé

//     stages{
//         stage('install dépendence'){
//             steps{
//                 sh 'node --version'
//                 sh 'npm --version'
//                 sh 'npm install'
//             }
//         }
//         stage('vérifier les pré-requis'){
//             steps{
//                script{
               
//                 if (params.run_with_tag==true)
//                 {
//                     sh "npx cypress run --browser ${params.browser} grepTags='${params.tag}'"
//                 }
//                 else {sh "npx cypress run --browser ${params.browser}"}

//                }
//             }
//         }
//     }


// }

pipeline {
    agent {
        docker {
            image 'cypress/included:cypress-15.11.0-node-24.14.0-chrome-145.0.7632.116-1-ff-148.0-edge-145.0.3800.70-1'
            args '--user=root --entrypoint=""'
        }
    }

    parameters {
        choice(
            name: 'tag',
            choices: ['@smoke', '@e2e'],
            description: 'Choisissez le tag à exécuter'
        )
        choice(
            name: 'browser',
            choices: ['chrome', 'firefox', 'edge'],
            description: 'Choisissez le navigateur'
        )
        booleanParam(
            name: 'run_with_tag',
            defaultValue: false,
            description: 'Lancer les tests avec ou sans tag'
        )
    }

    stages {
        stage('Vérifier les versions') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npx cypress --version'
            }
        }

        stage('Installer les dépendances') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Lancer les tests Cypress') {
            steps {
                script {
                    def cypressCmd = "npx cypress run --browser ${params.browser}"

                    if (params.run_with_tag) {
                        cypressCmd += " --env grepTags='${params.tag}'"
                    }

                    echo "Commande exécutée : ${cypressCmd}"
                    sh cypressCmd
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé.'
            junit testResults: 'cypress/results/*.xml', allowEmptyResults: true
            publishHTML(target: [
                allowMissing         : true,
                alwaysLinkToLastBuild: true,
                keepAll              : true,
                reportDir            : 'cypress/reports',
                reportFiles          : 'index.html',
                reportName           : 'Rapport Cypress'
            ])
        }
        success {
            echo '✅ Tous les tests ont réussi.'
        }
        failure {
            echo '❌ Des tests ont échoué. Consultez les rapports.'
        }
    }
}
