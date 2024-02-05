# EC2 Instance creation using CLI

aws ec2 help
aws ec2 run-instances \ 
 --image-id <ami-from-next-command> \
 --instance-type t2.micro

aws ec2 run-instances \
> --image-id ami-0f1a6835595fb9246 \
> --instance-type t2.micro \
> --block-device-mappings \
      '[{
        "DeviceName":"/dev/xvda",
        "Ebs":{
          "VolumeSize":10,
          "VolumeType":"gp2"
        }
      }]' \
  --tag-specifications \
      'ResourceType=instance,Tags=[{Key=Name,Value=ec2-kodekloud}]' \
> --subnet-id subnet-0d0c6b15b2749e0cb


aws ec2 describe-instances --region us-east-1 --query "Reservations[].Instances[].InstanceId"

# Terminate Instance

aws ec2 terminate-instances --instance-ids i-06231123ba532f69b

# Get Terminated Instance

aws ec2 describe-instances   --query 'Reservations[*].Instances[?State.Name == `terminated`].[InstanceId]'

# Get AMIs

aws ec2 describe images --owners self amazon
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn2-ami-hvm-2.0.*x86_64-gp2" --query 'Images[*].{ID:ImageId}' --output text

# Get VPC ID

aws ec2 describe-vpcs --filters Name=isDefault,Values=true --query 'Vpcs[0].VpcId' --output text

# Get Number of Subnets in a Region

aws ec2 describe-subnets --query 'length(Subnets[])' --region us-east-1

# Get subnet from a AZ 

aws ec2 describe-subnets --filters "Name=availability-zone,Values=us-east-1c" --query 'Subnets[].SubnetId' --region us-east-1

