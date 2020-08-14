git config --global user.name "barisguler"
git config --global user.email baris@alumni.psu.edu
more ~/.gitconfig 
# To cache the password for a long time
git config --global credential.helper 'cache --timeout=36000000'
git clone https://github.com/barisguler/qsworkshop.git
cd qsworkshop/
curl https://raw.githubusercontent.com/aws-quickstart/quickstart-workshop-labs/master/workshop-base/base.tar | tar -x
more .taskcat.yml 
ls -al templates/
cat  templates/workshop.template.yaml 
more ci/taskcat.yml 
more ci/workshop_input.json 

#
git add --all .
git log
git commit -a -m 'Load base content'
git push
Username for 'https://github.com/barisguler/qsworkshop.git': barisguler
Password for 'https://barisguler@github.com/barisguler/qsworkshop.git': 
Counting objects: 8, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (8/8), 883 bytes | 883.00 KiB/s, done.
Total 8 (delta 0), reused 0 (delta 0)
To https://github.com/barisguler/qsworkshop.git
 * [new branch]      master -> master

#Checkout master as development branch
git checkout -b develop
# push develop to remote and set upstream to it
git push --set-upstream origin develop


# Each modular quickstart is handled as git submodule
git submodule add -b master https://github.com/aws-quickstart/quickstart-aws-vpc.git submodules/quickstart-aws-vpc
# Push the changes into the develop branch
git commit -a -m "Add QuickStart VPC Submodule"

#If you need to update your submodules later you can use the following command 
git submodule update --recursive 

#Similarly, if you are cloning an existing repo that already contains a submodule you will need to run first
git submodule init 
# and then run the following to copy the contents
git submodule update -r 

# Push the changes to the git repo
git push origin develop

# Get all parameters for master template 
curl https://raw.githubusercontent.com/aws-quickstart/quickstart-workshop-labs/master/implementing/templates/incomplete.master.template.yaml -o templates/master.template.yaml
# Get example VPC stack details
curl -s https://raw.githubusercontent.com/aws-quickstart/quickstart-workshop-labs/master/implementing/templates/vpcstack.master.template.yaml >>templates/master.template.yaml
# Add and commit changes
git add .
git commit -a -m 'Add QuickStart VPC as a nested stack'

# Get workload example - Elastic Load Balancer
curl -s https://raw.githubusercontent.com/aws-quickstart/quickstart-workshop-labs/master/implementing/templates/workload.template.yaml -o templates/workload.template.yaml
# add and commit
git add templates/workload.template.yaml
git commit -a -m 'Added Workload'

# Add the workload to the master template
curl -s https://raw.githubusercontent.com/aws-quickstart/quickstart-workshop-labs/master/implementing/templates/webserver.master.template.yaml >>templates/master.template.yaml
# Commit changes
git commit -a -m 'Add Workload as a nested stack'

# Add outputs to the master template
curl -s https://raw.githubusercontent.com/aws-quickstart/quickstart-workshop-labs/master/implementing/templates/outputs.master.template.yaml >>templates/master.template.yaml
# Commit changes
git commit -a -m 'Add outputs to master template'
