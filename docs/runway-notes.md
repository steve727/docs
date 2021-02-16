## runway notes
https://docs.onica.com/projects/runway/en/latest/quickstart/index.html

```mkdir nice-project

cd nice-project

curl -L https://oni.ca/runway/latest/osx -o runway

chmod +x runway

git init

git checkout -b ENV-dev

./runway gen-sample cfngin

vim sampleapp.cfn/dev-us-east-1.env

./runway init

./runway gen-sample tf

vim sampleapp.tf/backend-us-east-1.tfvars

cfngin-steve-nice-us-east-1
    
./runway run-aws sts get-caller-identity

./runway tfenv install 0.14.6