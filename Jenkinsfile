node {
    def app

    // ========== CHANGES REQUIRED ========== //
    def gitUsername   = "write-your-github-username"    // 👈 REPLACE WITH YOUR GITHUB USERNAME
    def gitEmail      = "write-your-email"              // 👈 REPLACE WITH YOUR EMAIL
    def dockerhubUser = "write-your-dockerhub-username" // 👈 REPLACE WITH YOUR DOCKERHUB USERNAME
    // ====================================== //

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(
                    credentialsId: 'github', 
                    passwordVariable: 'GIT_PASSWORD', 
                    usernameVariable: 'GIT_USERNAME'
                )]) {
                    // Configure Git
                    sh "git config user.email '${gitEmail}'"
                    sh "git config user.name '${gitUsername}'"

                    // Update deployment.yaml with the user's Docker Hub image
                    sh "cat deployment.yaml"
                    sh "sed -i 's+${dockerhubUser}/test.*+${dockerhubUser}/test:${DOCKERTAG}+g' deployment.yaml"
                    sh "cat deployment.yaml"

                    // Commit and push changes
                    sh "git add ."
                    sh "git commit -m 'Updated by Jenkins Job: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                }
            }
        }
    }
}