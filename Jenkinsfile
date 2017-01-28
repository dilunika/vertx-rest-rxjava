node {

    stage('Checkout'){

        try {

            git 'https://github.com/dilunika/majan-admintools-api.git'
            echo 'Checked out repository.'

        } catch (Exception e) {

            def message = 'Failed to checkout from repository. ' + e.message
            notifyBuildBreaks(message)
            throw e
        }
    }

    stage('Unit Tests'){
        sh './gradlew clean test'
        junit 'build/test-results/junit-platform/TEST-*.xml'
    }

}

def notifyBuildBreaks(errorMessage) {

    def commiter = sh (
            script: 'git --no-pager show -s --format=\'%ae\'',
            returnStdout: true
    ).trim()

    currentBuild.result = 'FAILURE'
    def message = 'Majan Admin Tools API Build Failed. Last Commit is done by : ' + commiter + ' and reason is - ' + errorMessage + '. Visit ' + currentBuild.absoluteUrl;
    sendSlack('#FF0000', message)
}

def sendSlack(color, message) {

    slackSend channel: '#api-delivery',
            color: color,
            message: message,
            teamDomain: 'majan',
            tokenCredentialId: 'majanslack'
}