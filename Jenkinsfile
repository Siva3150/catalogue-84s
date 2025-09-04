// @Library ('jenkins-shared-library-84s')

// def configMap = [
//     project : "roboshop",
//     component : "catalogue"
// ]

// if(  env.BRANCH_NAME.equalsIgnoreCase('main') ){ // if not equals to main
//     nodejsEKSPipeline(configMap) // by default it will call, call function inside this pipeline
// }
// else{
//     echo "Please proceed with PROD process"
// }

@Library('jenkins-shared-library-84s') 

def configMap = [
    project  : 'roboshop',
    component: 'catalogue'
]

if ((env.BRANCH_NAME ?: '').equalsIgnoreCase('main')) {
    echo 'Please proceed with PROD process'
} else {
    nodejsEKSpipeline(configMap)
}


 



// @Library ('jenkins-shared-library-84s')

// def configMap = [
//     greeting : "Hello jenkins"
// ]

//  samplePipeline(configMap) // by default it will call, call function inside this pipeline

