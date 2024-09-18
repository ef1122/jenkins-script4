

node('dev')
{   
   def cardNumber=input message: 'Introduzca el número de tarjeta', 
                        parameters: [string(defaultValue: '4111111111111111', 
                        name: 'cardNumber')]
   stage('Chceck Card Number'){
      if (cardNumber.length() != 16 ){
        error 'Invalid card number: The card number must be exactly 16 characters long.'
      }

   }
   stage('Get GIT repository')
   {
    git branch: 'main', url: 'https://github.com/ApasoftTraining/jenkins-script3.git'
   }
   stage('Compile'){
    sh 'mvn compile'
   }
   stage('Test'){
    sh 'mvn test'
   }
   stage('Execute program'){
    echo 'Executing then Java program'
    sh "mvn exec:java -Dexec.mainClass='com.apasoft.CardProcessor' -Dexec.args='${cardNumber}'"
    stash includes: 'target/**', name: 'target-jar'
   }
}
node('prod'){
    stage('Deploy') {
        //We unpack the target on the new node
        unstash 'target-jar'
        echo 'Deploying the application to production........'
        // We copy the artifact to the /home/jenkins/app directory on the production node
        sh 'rm -rf  /home/jenkins/jenkins-app/'
        sh 'mkdir /home/jenkins/jenkins-app/'
        sh 'cp -r target/* /home/jenkins/jenkins-app/'
        echo 'Deployment completed!'
    }
}