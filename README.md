# kops

kops stands for kubernetes operation, it is currently the best tool to deploy kubernetes cluster on the cloud. Kops is officially supported by AWS, with the help of kops we can create, upgrade maintain the production grade kubernetes cluster from CLI.



## Creating first cluster using kops

###  Prerequisite:
  AWS CLI and kubectl is required
 ###  Installing kops in Linux:
        curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
        chmod +x kops-linux-amd64
        sudo mv kops-linux-amd64 /usr/local/bin/kops
 ###  Route53  hosted zone:
 kops requires DNS so that it can reach to Kubernetes API server from clients. DNS name also act as the cluster name so make sure to use valid DNS name. As in my example I used nikhils.com as my hosted zone, so api.nikhils.com will be the endpoint of API server.
 #### Create private hosted zone:
 
        Go to AWS console and search for Route53
        Go to DNS management by clicking on "get started now".
        Create a private hosted zone there.
        And choose any region for vpc, in my case I used US East.
                   
 
   ### S3 bucket:
   After the creation of cluster all of its information is stored into S3 bucket including the configurations and the key it is using, kops manage the cluster by using these information from the S3 bucket.
   #### Command to create S3 bucket:
         aws s3api create-bucket --bucket < bucket-name >  --region  < vpc-region name>
         For example: aws s3api create-bucket --bucket nikhils-com-state-store --region us-east-1
         Note: Make sure that hosted zone and S3 bucket should be in same vpc region.
   ### Cluster Creation:
         First create environment variable of S3 bucket and cluster name:
              export NAME= < your private hosted zone name> 
              for example: export NAME=nikhils.com
              export KOPS_STATE_STORE=s3://< your bucket name>
              for example: export KOPS_STATE_STORE=s3://nikhils-com-state-store
         Now run command "kops create cluster $NAME --dns private --topology private  --networking calico  --state $KOPS_STATE_STORE --zones us-east-1c  --yes" to create a cluster.
         Run command "kops validate cluster $NAME" to validate your cluster
   ### kops commands to explore cluster:
         kops get cluster:  List the all clusters
         kops edit cluster <clustername>: To edit the cluster
         kops update cluster <clustername>: To update the cluster 
         
  ## Delete cluster:
    You can delete the cluster by running the command "kops delete cluster --name  $NAME --yes"

      

   
               
               
    
           


