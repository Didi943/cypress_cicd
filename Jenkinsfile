pipeline {
     agent any
    // {
    //     // Environnrement Node, npm, navigateur chromium, git


    // }

    // paramètre pour ajouter les tags et rapport
    // tags
    // rapport
    parameters{
        choice(name:"tag",choices:["@smoke","@e2e"],description:"choisissez le tag")
        choice(name:"browser",choices:["chromium","firefox","edge"],description:"choisissez le navigateur")
    }
    // browser if browser == chromium on lance les testes sinon on affiche aucun test est lancé

    stages{
        stage('vérifier les pré-requis'){
            steps{
                sh ' echo "lancement" '
            }
        }
    }


}