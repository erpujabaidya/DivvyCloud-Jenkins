pipeline {
   agent any
 
   stages {
       stage('GitHub checkout'){
           steps{
       git branch: 'master', credentialsId: '448b0998-a41f-4d90-b417-7ed8d1382b3e', url: 'https://github.com/santoshreddyspy13/DivvyCloud.git'
   }
       }
       stage('Generate Terraform Plan') {
            steps {
              sh 'terraform init DivvyCloudTF/' 
                sh 'terraform plan -out DivvyCloudTF/tf.plan DivvyCloudTF/'
                sh 'terraform show -json DivvyCloudTF/tf.plan > DivvyCloudTF/tf.plan.json'
               stash includes: 'DivvyCloudTF/tf.plan.json', name: 'divvycloud-iac-security-stash'
            }
        } 
        stage('Submit Terraform Plan to DivvyCloud') {
            steps 
            {
                withCredentials([usernamePassword(credentialsId: 'dded578a-af9a-40c8-8865-d06ad62263e9', usernameVariable: 'DIVVY_USERNAME', passwordVariable: 'DIVVY_PASSWORD')]) {
                unstash 'divvycloud-iac-security-stash'
                script {
                    try {
                     sh 'python3 /home/ubuntu/iac_api.py https://mindtree-pov.divvycloud.net ConfigurationJenkins DivvyCloudTF/tf.plan.json --html_out scan_output.html' 
                     sh 'python3 /home/ubuntu/iac_api.py https://mindtree-pov.divvycloud.net IACScanningfromJenkins DivvyCloudTF/tf.plan.json --json_out scan_output.json' 


                    } catch (e) {
                        throw e
                    }  finally {
                        archiveArtifacts 'scan_output.*'   
                    }
                }
                }
            }
        }
    }
}
