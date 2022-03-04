### [Enable OS Login](https://cloud.google.com/compute/docs/instances/managing-instance-access#gcloud)
```bash
gcloud compute project-info add-metadata \
    --metadata enable-oslogin=TRUE

gcloud compute os-login ssh-keys add \
    --key-file=/Users/steve/.ssh/id_rsa.pub
    
ssh -i id_rsa username@ipaddress
```
