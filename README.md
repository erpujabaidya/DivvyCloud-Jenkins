# DivvyCloud-Jenkins
#Execute Shell
# Generate a Terraform plan and convert it to JSON
cd /var/lib/jenkins/workspace/divvycloud_IAC_scan_amit/DivvyCloudTF
terraform init
terraform plan -out tf.plan
terraform show -json tf.plan > tf.plan.json


# Run our IaC script and configure it according to the docstrings in the script.
python3 /home/ubuntu/iac_api.py https://mindtree-pov.divvycloud.net ConfigurationJenkins /var/lib/jenkins/workspace/divvycloud_IAC_scan_amit/DivvyCloudTF/tf.plan.json --html_out scan_output.html --json_out scan_output.json

# For python Script visit
https://bitbucket.org/!api/2.0/snippets/divvycloud/LryE9B/HEAD/files/iac_api.py

#Specify the bindings in Jenkins project And put the git repo url https://github.com/santoshreddyspy13/DivvyCloud.git
or, visit https://docs.divvycloud.com/docs/using-the-iac-analyzer-via-api


