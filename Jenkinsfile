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
        booleanParam(name:"run_with_tag",defaultValue:false,description:"lancer avec ou sans le tag")
    }
    // browser if browser == chromium on lance les testes sinon on affiche aucun test est lancé

    stages{
        stage('vérifier les pré-requis'){
            steps{
               script{
                if(params.browser=="chromium"){
                    if (params.run_with_tag==true)
                    {
                        sh "npx cypress run --browser ${params.browser} grepTags='${params.tag}'"
                    }
                    else {sh "npx cypress run --browser ${params.browser}"}
                
                 }
                 else { sh "echo 'aucun test est lancer' "}
                 
               }
            }
        }
    }


}