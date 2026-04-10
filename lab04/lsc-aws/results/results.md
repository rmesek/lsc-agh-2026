```bash
$ bash deploy/06-workstation.sh
Config loaded: ACCOUNT_ID=379102673713, REGION=us-east-1
=== Step 6: EC2 Workstation (t3.small) ===
Using pre-created key pair: vockey
NOTE: Download the PEM file from 'AWS Details' in the Learner Lab console.
  Save it as ~/.ssh/labsuser.pem and run: chmod 600 ~/.ssh/labsuser.pem
Creating workstation security group...
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-0eba23dc5ea0d4ac6",
            "GroupId": "sg-0cbd93614da4d8bf4",
            "GroupOwnerId": "379102673713",
            "IsEgress": false,
            "IpProtocol": "tcp",
            "FromPort": 22,
            "ToPort": 22,
            "CidrIpv4": "0.0.0.0/0",
            "SecurityGroupRuleArn": "arn:aws:ec2:us-east-1:379102673713:security-group-rule/sgr-0eba23dc5ea0d4ac6"
        }
    ]
}
Security Group: sg-0cbd93614da4d8bf4
Repo to clone on workstation: https://github.com/rmesek/lsc-agh-2026
Launching workstation...
Instance ID: i-09fd71d70d5059b0f
Waiting for instance to be running...
=== Workstation ready. Public IP: 44.211.73.214 ===
SSH: ssh -i /home/fractal/.ssh/labsuser.pem ec2-user@44.211.73.214
NOTE: Wait ~2 minutes for user-data to complete, then:
  ssh -i /home/fractal/.ssh/labsuser.pem ec2-user@44.211.73.214
  cd lsc-agh-2026 && oha --version && docker --version
```

```bash
$ bash deploy/01-ecr.sh
Config loaded: ACCOUNT_ID=379102673713, REGION=us-east-1
=== Step 1: ECR Repository & Docker Image ===
Creating ECR repository...
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-1:379102673713:repository/lsc-knn-app",
        "registryId": "379102673713",
        "repositoryName": "lsc-knn-app",
        "repositoryUri": "379102673713.dkr.ecr.us-east-1.amazonaws.com/lsc-knn-app",
        "createdAt": "2026-04-09T23:13:03.118000+00:00",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}
Logging into ECR...
WARNING! Your password will be stored unencrypted in /home/ec2-user/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
Building Docker image...
[+] Building 15.4s (11/11) FINISHED                                                                               docker:default
 => [internal] load build definition from Dockerfile                                                                        0.0s
 => => transferring dockerfile: 540B                                                                                        0.0s
 => [internal] load metadata for public.ecr.aws/lambda/python:3.12                                                          0.3s
 => [internal] load .dockerignore                                                                                           0.0s
 => => transferring context: 2B                                                                                             0.0s
 => [1/6] FROM public.ecr.aws/lambda/python:3.12@sha256:e6f5e8e6607c461c49488f2e02204960e64054034dca27653bb998c78a194597    9.4s
 => => resolve public.ecr.aws/lambda/python:3.12@sha256:e6f5e8e6607c461c49488f2e02204960e64054034dca27653bb998c78a194597    0.0s
 => => sha256:c55d39a838d339d7040dc33523b5ffe3bf4aa63dec6c46243e31762c8887e9f3 3.77MB / 3.77MB                              0.2s
 => => sha256:2379caf5d200ad048775318acbdd9479139bc5e2f36e41704970e1b139234e83 146.08MB / 146.08MB                          1.5s
 => => sha256:c03298f16942f759cc6d23dcd088dec26f4497397891b90cf6d57ce4aced4526 4.63kB / 4.63kB                              0.0s
 => => sha256:8d724fab0c61aa27e701c33d2d2a53c6e709cdd7abe2e4dc8ebb8fced80a728d 88.13kB / 88.13kB                            0.1s
 => => sha256:f7b9bc85ab421e729bc6d0442935e3ba1e8d31f851272463b91d16223d21093f 417B / 417B                                  0.1s
 => => sha256:e6f5e8e6607c461c49488f2e02204960e64054034dca27653bb998c78a194597 772B / 772B                                  0.0s
 => => sha256:c7a73fd6b98bb3b97df4dafd2ba87803b3942443ef9811175e844434ae85129e 1.58kB / 1.58kB                              0.0s
 => => sha256:bf639fcde9663382fc2fd50df00b201a612144ac9efcfc6d1b3615841314b821 38.70MB / 38.70MB                            0.3s
 => => sha256:80727638ed126198366be086d0f6f0078f279beb1fe3acbc83f315531a603013 14.20kB / 14.20kB                            0.2s
 => => extracting sha256:bf639fcde9663382fc2fd50df00b201a612144ac9efcfc6d1b3615841314b821                                   2.2s
 => => extracting sha256:8d724fab0c61aa27e701c33d2d2a53c6e709cdd7abe2e4dc8ebb8fced80a728d                                   0.0s
 => => extracting sha256:f7b9bc85ab421e729bc6d0442935e3ba1e8d31f851272463b91d16223d21093f                                   0.0s
 => => extracting sha256:c55d39a838d339d7040dc33523b5ffe3bf4aa63dec6c46243e31762c8887e9f3                                   0.1s
 => => extracting sha256:2379caf5d200ad048775318acbdd9479139bc5e2f36e41704970e1b139234e83                                   6.3s
 => => extracting sha256:80727638ed126198366be086d0f6f0078f279beb1fe3acbc83f315531a603013                                   0.0s
 => [internal] load build context                                                                                           0.0s
 => => transferring context: 3.86kB                                                                                         0.0s
 => [2/6] COPY requirements.txt /var/task/                                                                                  0.3s
 => [3/6] RUN pip install --no-cache-dir -r /var/task/requirements.txt                                                      4.3s
 => [4/6] COPY app.py handler.py generate_dataset.py /var/task/                                                             0.1s
 => [5/6] COPY entrypoint.sh /entrypoint.sh                                                                                 0.0s
 => [6/6] RUN chmod +x /entrypoint.sh                                                                                       0.3s
 => exporting to image                                                                                                      0.5s
 => => exporting layers                                                                                                     0.5s
 => => writing image sha256:b409da9d88e907c716bde650297ab3e276b4c070ae9172306b1d75a081f5fdf5                                0.0s
 => => naming to docker.io/library/lsc-knn-app:latest                                                                       0.0s
Pushing image to ECR...
The push refers to repository [379102673713.dkr.ecr.us-east-1.amazonaws.com/lsc-knn-app]
d6ddb74180c2: Pushed 
f7358cec3dad: Pushed 
0a9dde9cccde: Pushed 
f2b6dbc9ae37: Pushed 
dba4f5d2f223: Pushed 
546e739417c1: Pushed 
0553e8c2427a: Pushed 
b2fe80193d67: Pushed 
06b83f802e1c: Pushed 
3f0c7b8a5449: Pushed 
40c41abed735: Pushed 
latest: digest: sha256:0be19c774c24bab29baa26c9d9b675deb19b72867aedf3d4fd59cc624aa966fe size: 2619
=== ECR done. Image URI: 379102673713.dkr.ecr.us-east-1.amazonaws.com/lsc-knn-app:latest ===
```

```bash
$ bash deploy/02-lambda-zip.sh
Config loaded: ACCOUNT_ID=379102673713, REGION=us-east-1
=== Step 2: Lambda Zip Deployment ===
Building NumPy Lambda layer...
Unable to find image 'python:3.12-slim' locally
3.12-slim: Pulling from library/python
5435b2dcdf5c: Pull complete 
25981ed25cff: Pull complete 
d4c207a1ca27: Pull complete 
f0bdb572205e: Pull complete 
Digest: sha256:804ddf3251a60bbf9c92e73b7566c40428d54d0e79d3428194edf40da6521286
Status: Downloaded newer image for python:3.12-slim
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.

[notice] A new release of pip is available: 25.0.1 -> 26.0.1
[notice] To update, run: pip install --upgrade pip
Publishing Lambda layer...
Layer ARN: arn:aws:lambda:us-east-1:379102673713:layer:numpy-py312:1
Packaging Lambda zip...
  adding: handler.py (deflated 51%)
  adding: app.py (deflated 49%)
  adding: generate_dataset.py (deflated 28%)
Creating Lambda function (zip)...
arn:aws:lambda:us-east-1:379102673713:function:lsc-knn-zip
Waiting for function to become active...
Creating Function URL...
{
    "Statement": "{\"Sid\":\"FunctionURLInvoke\",\"Effect\":\"Allow\",\"Principal\":\"*\",\"Action\":\"lambda:InvokeFunctionUrl\",\"Resource\":\"arn:aws:lambda:us-east-1:379102673713:function:lsc-knn-zip\",\"Condition\":{\"StringEquals\":{\"lambda:FunctionUrlAuthType\":\"AWS_IAM\"}}}"
}
=== Lambda Zip done. Function URL: https://llusni53oqeadbwebyb2owe2hu0utrua.lambda-url.us-east-1.on.aws/ ===
```

```bash
$ bash deploy/03-lambda-container.sh
Config loaded: ACCOUNT_ID=379102673713, REGION=us-east-1
=== Step 3: Lambda Container Image Deployment ===
Creating Lambda function (container)...
arn:aws:lambda:us-east-1:379102673713:function:lsc-knn-container
Waiting for function to become active...
Creating Function URL...
{
    "Statement": "{\"Sid\":\"FunctionURLInvoke\",\"Effect\":\"Allow\",\"Principal\":\"*\",\"Action\":\"lambda:InvokeFunctionUrl\",\"Resource\":\"arn:aws:lambda:us-east-1:379102673713:function:lsc-knn-container\",\"Condition\":{\"StringEquals\":{\"lambda:FunctionUrlAuthType\":\"AWS_IAM\"}}}"
}
=== Lambda Container done. Function URL: https://mmc6my4yehyr7fvjlndmvq4sri0wgnbt.lambda-url.us-east-1.on.aws/ ===
```

```bash
$ bash deploy/04-fargate.sh
Config loaded: ACCOUNT_ID=379102673713, REGION=us-east-1
=== Step 4: ECS Fargate with ALB ===
Finding default VPC...
VPC: vpc-06a8131318ba228a0
Finding subnets (need 2+ AZs for ALB)...
Subnets: subnet-0a9dc53d0d27816dd, subnet-0cfc1111a39b5a303
Creating ALB security group...
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-073814ac6d58d2168",
            "GroupId": "sg-015aa614b41b66a86",
            "GroupOwnerId": "379102673713",
            "IsEgress": false,
            "IpProtocol": "tcp",
            "FromPort": 80,
            "ToPort": 80,
            "CidrIpv4": "0.0.0.0/0",
            "SecurityGroupRuleArn": "arn:aws:ec2:us-east-1:379102673713:security-group-rule/sgr-073814ac6d58d2168"
        }
    ]
}
ALB SG: sg-015aa614b41b66a86
Creating task security group...
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-04f2b49abc37be860",
            "GroupId": "sg-033c993343f1cea46",
            "GroupOwnerId": "379102673713",
            "IsEgress": false,
            "IpProtocol": "tcp",
            "FromPort": 8080,
            "ToPort": 8080,
            "ReferencedGroupInfo": {
                "GroupId": "sg-015aa614b41b66a86",
                "UserId": "379102673713"
            },
            "SecurityGroupRuleArn": "arn:aws:ec2:us-east-1:379102673713:security-group-rule/sgr-04f2b49abc37be860"
        }
    ]
}
...skipping...
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-04f2b49abc37be860",
            "GroupId": "sg-033c993343f1cea46",
            "GroupOwnerId": "379102673713",
            "IsEgress": false,
            "IpProtocol": "tcp",
            "FromPort": 8080,
            "ToPort": 8080,
            "ReferencedGroupInfo": {
                "GroupId": "sg-015aa614b41b66a86",
                "UserId": "379102673713"
            },
            "SecurityGroupRuleArn": "arn:aws:ec2:us-east-1:379102673713:security-group-rule/sgr-04f2b49abc37be860"
        }
    ]
}
Task SG: sg-033c993343f1cea46
Creating ECS cluster...
arn:aws:ecs:us-east-1:379102673713:cluster/lsc-knn-cluster
Creating log group...
Registering task definition...
Task definition: arn:aws:ecs:us-east-1:379102673713:task-definition/lsc-knn-task:1
Creating ALB...
ALB ARN: arn:aws:elasticloadbalancing:us-east-1:379102673713:loadbalancer/app/lsc-knn-alb/3cd5b3a8b8f68869
ALB DNS: lsc-knn-alb-567435643.us-east-1.elb.amazonaws.com
Creating target group...
Target Group: arn:aws:elasticloadbalancing:us-east-1:379102673713:targetgroup/lsc-knn-tg/41fc0f9e289bafa9
Creating ALB listener...
Listener: arn:aws:elasticloadbalancing:us-east-1:379102673713:listener/app/lsc-knn-alb/3cd5b3a8b8f68869/1d6d46e1fbab56ea
Creating ECS service...
arn:aws:ecs:us-east-1:379102673713:service/lsc-knn-cluster/lsc-knn-service
Waiting for ECS service to stabilize (this may take 1-2 minutes)...
=== Fargate done. ALB URL: http://lsc-knn-alb-567435643.us-east-1.elb.amazonaws.com ===
```

```bash
$ curl -X POST -H "Content-Type: application/json" -d @loadtest/query.json \
    http://lsc-knn-alb-567435643.us-east-1.elb.amazonaws.com/search
{"instance_id":"ip-172-31-37-185.ec2.internal","query_time_ms":22.43,"results":[{"distance":12.001459121704102,"index":35859},{"distance":12.059946060180664,"index":24682},{"distance":12.487079620361328,"index":35397},{"distance":12.489519119262695,"index":20160},{"distance":12.499402046203613,"index":30454}]}
```

```bash
$ bash deploy/05-ec2-app.sh
Config loaded: ACCOUNT_ID=379102673713, REGION=us-east-1
=== Step 5: EC2 Application Instance (t3.small) ===
Creating app security group...
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-098658553b5973177",
            "GroupId": "sg-0a5364e84b421b326",
            "GroupOwnerId": "379102673713",
            "IsEgress": false,
            "IpProtocol": "tcp",
            "FromPort": 8080,
            "ToPort": 8080,
            "CidrIpv4": "0.0.0.0/0",
            "SecurityGroupRuleArn": "arn:aws:ec2:us-east-1:379102673713:security-group-rule/sgr-098658553b5973177"
        }
    ]
}
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-0d891d7442fb56c3b",
            "GroupId": "sg-0a5364e84b421b326",
            "GroupOwnerId": "379102673713",
            "IsEgress": false,
            "IpProtocol": "tcp",
            "FromPort": 22,
            "ToPort": 22,
            "CidrIpv4": "0.0.0.0/0",
            "SecurityGroupRuleArn": "arn:aws:ec2:us-east-1:379102673713:security-group-rule/sgr-0d891d7442fb56c3b"
        }
    ]
}
Security Group: sg-0a5364e84b421b326
Finding latest Amazon Linux 2023 AMI...
AMI: ami-0ea87431b78a82070
Checking for LabInstanceProfile...
Instance Profile: LabInstanceProfile
Launching EC2 instance...
Instance ID: i-0483c0cd690499ac0
Waiting for instance to be running...
=== EC2 App done. Public IP: 44.222.126.173 ===
URL: http://44.222.126.173:8080
NOTE: Wait ~2 minutes for Docker to pull and start the container.
Test with: curl -X POST -H 'Content-Type: application/json' -d @loadtest/query.json http://44.222.126.173:8080/search
```

```bash
$ curl -X POST -H 'Content-Type: application/json' -d @loadtest/query.json http://44.222.126.173:8080/search
{"instance_id":"be627ab38872","query_time_ms":33.598,"results":[{"distance":12.001459121704102,"index":35859},{"distance":12.059946060180664,"index":24682},{"distance":12.487079620361328,"index":35397},{"distance":12.489519119262695,"index":20160},{"distance":12.499402046203613,"index":30454}]}
```

```bash
$ bash loadtest/scenario-a.sh "$LAMBDA_ZIP_URL" "$LAMBDA_CONTAINER_URL"
=== Scenario A: Lambda Cold Start Characterization ===
Ensure Lambda has been idle for 20+ minutes before running.

--- Lambda Zip (30 sequential requests, 1/sec) ---
Summary:
  Success rate: 100.00%
  Total:        30058.0482 ms
  Slowest:      82.4414 ms
  Fastest:      15.9913 ms
  Average:      21.3514 ms
  Requests/sec: 0.9981

  Total data:   1.99 KiB
  Size/request: 68 B
  Size/sec:     67 B

Response time histogram:
  15.991 ms [1]  |■
  22.636 ms [24] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  29.281 ms [4]  |■■■■■
  35.926 ms [0]  |
  42.571 ms [0]  |
  49.216 ms [0]  |
  55.861 ms [0]  |
  62.506 ms [0]  |
  69.151 ms [0]  |
  75.796 ms [0]  |
  82.441 ms [1]  |■

Response time distribution:
  10.00% in 16.5386 ms
  25.00% in 17.2894 ms
  50.00% in 17.8641 ms
  75.00% in 20.7443 ms
  90.00% in 25.7975 ms
  95.00% in 28.8387 ms
  99.00% in 82.4414 ms
  99.90% in 82.4414 ms
  99.99% in 82.4414 ms


Details (average, fastest, slowest):
  DNS+dialup:   51.3260 ms, 51.3260 ms, 51.3260 ms
  DNS-lookup:   0.0848 ms, 0.0848 ms, 0.0848 ms

Status code distribution:
  [403] 30 responses

Waiting 20 minutes before container variant (for cold start reset)...
Press Ctrl+C to skip the wait if running variants separately.
Sleeping 1200s (20 min)...
```

```bash
$ bash loadtest/scenario-b.sh "$LAMBDA_ZIP_URL" "$LAMBDA_CONTAINER_URL" "$FARGATE_URL" "$EC2_URL"
=== Scenario B: Warm Steady-State Throughput ===

NOTE: Lambda concurrency is capped at 10 (AWS Academy limit: max 10 concurrent
Lambda execution environments). Fargate/EC2 use c=10 and c=50.

--- Warm-up phase ---
  Warming up Lambda Zip (60 requests at c=10)...
  Warming up Lambda Container (60 requests at c=10)...
  Warming up Fargate (60 requests at c=50)...
  Warming up EC2 (60 requests at c=50)...
Warm-up complete.

=== lambda-zip | concurrency=5 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        1613.7380 ms
  Slowest:      73.2587 ms
  Fastest:      7.2878 ms
  Average:      16.0050 ms
  Requests/sec: 309.8396

  Total data:   33.20 KiB
  Size/request: 68 B
  Size/sec:     20.58 KiB

Response time histogram:
   7.288 ms [1]   |
  13.885 ms [168] |■■■■■■■■■■■■■■■■■■
  20.482 ms [296] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  27.079 ms [26]  |■■
  33.676 ms [4]   |
  40.273 ms [0]   |
  46.870 ms [0]   |
  53.467 ms [0]   |
  60.065 ms [0]   |
  66.662 ms [0]   |
  73.259 ms [5]   |

Response time distribution:
  10.00% in 10.3396 ms
  25.00% in 11.9701 ms
  50.00% in 16.0406 ms
  75.00% in 17.9873 ms
  90.00% in 19.8171 ms
  95.00% in 21.6761 ms
  99.00% in 67.2585 ms
  99.90% in 73.2587 ms
  99.99% in 73.2587 ms


Details (average, fastest, slowest):
  DNS+dialup:   52.2201 ms, 51.2933 ms, 53.3320 ms
  DNS-lookup:   0.0179 ms, 0.0085 ms, 0.0521 ms

Status code distribution:
  [403] 500 responses

=== lambda-zip | concurrency=10 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        702.7358 ms
  Slowest:      73.9578 ms
  Fastest:      7.3103 ms
  Average:      13.8791 ms
  Requests/sec: 711.5049

  Total data:   33.20 KiB
  Size/request: 68 B
  Size/sec:     47.25 KiB

Response time histogram:
   7.310 ms [1]   |
  13.975 ms [361] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  20.640 ms [112] |■■■■■■■■■
  27.305 ms [12]  |■
  33.969 ms [3]   |
  40.634 ms [1]   |
  47.299 ms [0]   |
  53.964 ms [0]   |
  60.628 ms [0]   |
  67.293 ms [6]   |
  73.958 ms [4]   |

Response time distribution:
  10.00% in 9.5369 ms
  25.00% in 10.4664 ms
  50.00% in 11.6564 ms
  75.00% in 14.6853 ms
  90.00% in 18.3025 ms
  95.00% in 21.0848 ms
  99.00% in 67.2288 ms
  99.90% in 73.9578 ms
  99.99% in 73.9578 ms


Details (average, fastest, slowest):
  DNS+dialup:   52.8356 ms, 51.7810 ms, 53.8669 ms
  DNS-lookup:   0.0181 ms, 0.0079 ms, 0.0775 ms

Status code distribution:
  [403] 500 responses

=== lambda-container | concurrency=5 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        1542.0110 ms
  Slowest:      74.1799 ms
  Fastest:      8.1620 ms
  Average:      15.3092 ms
  Requests/sec: 324.2519

  Total data:   33.20 KiB
  Size/request: 68 B
  Size/sec:     21.53 KiB

Response time histogram:
   8.162 ms [1]   |
  14.764 ms [244] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  21.366 ms [233] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  27.967 ms [14]  |■
  34.569 ms [2]   |
  41.171 ms [0]   |
  47.773 ms [0]   |
  54.375 ms [0]   |
  60.976 ms [0]   |
  67.578 ms [2]   |
  74.180 ms [4]   |

Response time distribution:
  10.00% in 10.0396 ms
  25.00% in 11.5240 ms
  50.00% in 15.0143 ms
  75.00% in 17.2928 ms
  90.00% in 19.1052 ms
  95.00% in 20.8392 ms
  99.00% in 66.0788 ms
  99.90% in 74.1799 ms
  99.99% in 74.1799 ms


Details (average, fastest, slowest):
  DNS+dialup:   52.0425 ms, 51.4186 ms, 52.8209 ms
  DNS-lookup:   0.0181 ms, 0.0079 ms, 0.0507 ms

Status code distribution:
  [403] 500 responses

=== lambda-container | concurrency=10 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        695.2311 ms
  Slowest:      70.6975 ms
  Fastest:      7.0105 ms
  Average:      13.7171 ms
  Requests/sec: 719.1853

  Total data:   33.20 KiB
  Size/request: 68 B
  Size/sec:     47.76 KiB

Response time histogram:
   7.010 ms [1]   |
  13.379 ms [344] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  19.748 ms [125] |■■■■■■■■■■■
  26.117 ms [17]  |■
  32.485 ms [3]   |
  38.854 ms [0]   |
  45.223 ms [0]   |
  51.591 ms [0]   |
  57.960 ms [0]   |
  64.329 ms [2]   |
  70.698 ms [8]   |

Response time distribution:
  10.00% in 9.4479 ms
  25.00% in 10.4142 ms
  50.00% in 11.6449 ms
  75.00% in 14.4496 ms
  90.00% in 17.7335 ms
  95.00% in 20.2908 ms
  99.00% in 67.2208 ms
  99.90% in 70.6975 ms
  99.99% in 70.6975 ms


Details (average, fastest, slowest):
  DNS+dialup:   53.0465 ms, 52.0112 ms, 54.1394 ms
  DNS-lookup:   0.0150 ms, 0.0082 ms, 0.0577 ms

Status code distribution:
  [403] 500 responses

=== fargate | concurrency=10 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        40.3890 sec
  Slowest:      1.2076 sec
  Fastest:      0.2998 sec
  Average:      0.8043 sec
  Requests/sec: 12.3796

  Total data:   153.26 KiB
  Size/request: 313 B
  Size/sec:     3.79 KiB

Response time histogram:
  0.300 sec [1]   |
  0.391 sec [2]   |
  0.481 sec [3]   |
  0.572 sec [18]  |■■■■
  0.663 sec [55]  |■■■■■■■■■■■■■
  0.754 sec [103] |■■■■■■■■■■■■■■■■■■■■■■■■■
  0.844 sec [127] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.935 sec [105] |■■■■■■■■■■■■■■■■■■■■■■■■■■
  1.026 sec [51]  |■■■■■■■■■■■■
  1.117 sec [28]  |■■■■■■■
  1.208 sec [7]   |■

Response time distribution:
  10.00% in 0.6042 sec
  25.00% in 0.7005 sec
  50.00% in 0.8000 sec
  75.00% in 0.9004 sec
  90.00% in 0.9998 sec
  95.00% in 1.0893 sec
  99.00% in 1.1798 sec
  99.90% in 1.2076 sec
  99.99% in 1.2076 sec


Details (average, fastest, slowest):
  DNS+dialup:   0.0007 sec, 0.0006 sec, 0.0009 sec
  DNS-lookup:   0.0000 sec, 0.0000 sec, 0.0001 sec

Status code distribution:
  [200] 500 responses

=== fargate | concurrency=50 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        40.7846 sec
  Slowest:      4.8526 sec
  Fastest:      0.5720 sec
  Average:      3.8977 sec
  Requests/sec: 12.2595

  Total data:   153.27 KiB
  Size/request: 313 B
  Size/sec:     3.76 KiB

Response time histogram:
  0.572 sec [1]   |
  1.000 sec [8]   |■
  1.428 sec [5]   |
  1.856 sec [3]   |
  2.284 sec [9]   |■
  2.712 sec [4]   |
  3.140 sec [5]   |
  3.568 sec [6]   |
  3.997 sec [176] |■■■■■■■■■■■■■■■■■■■■■■
  4.425 sec [248] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  4.853 sec [35]  |■■■■

Response time distribution:
  10.00% in 3.6942 sec
  25.00% in 3.8917 sec
  50.00% in 4.0037 sec
  75.00% in 4.1505 sec
  90.00% in 4.3184 sec
  95.00% in 4.5046 sec
  99.00% in 4.7023 sec
  99.90% in 4.8526 sec
  99.99% in 4.8526 sec


Details (average, fastest, slowest):
  DNS+dialup:   0.0013 sec, 0.0008 sec, 0.0020 sec
  DNS-lookup:   0.0000 sec, 0.0000 sec, 0.0001 sec

Status code distribution:
  [200] 500 responses

=== ec2 | concurrency=10 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        12945.9866 ms
  Slowest:      471.3955 ms
  Fastest:      117.9015 ms
  Average:      257.8332 ms
  Requests/sec: 38.6220

  Total data:   144.87 KiB
  Size/request: 296 B
  Size/sec:     11.19 KiB

Response time histogram:
  117.901 ms [1]   |
  153.251 ms [6]   |■
  188.600 ms [31]  |■■■■■■■
  223.950 ms [89]  |■■■■■■■■■■■■■■■■■■■■
  259.299 ms [136] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  294.648 ms [126] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  329.998 ms [72]  |■■■■■■■■■■■■■■■■
  365.347 ms [29]  |■■■■■■
  400.697 ms [6]   |■
  436.046 ms [2]   |
  471.396 ms [2]   |

Response time distribution:
  10.00% in 195.9275 ms
  25.00% in 223.3409 ms
  50.00% in 254.8800 ms
  75.00% in 290.4438 ms
  90.00% in 324.3660 ms
  95.00% in 344.9744 ms
  99.00% in 399.6359 ms
  99.90% in 471.3955 ms
  99.99% in 471.3955 ms


Details (average, fastest, slowest):
  DNS+dialup:   0.3156 ms, 0.2031 ms, 0.5332 ms
  DNS-lookup:   0.0080 ms, 0.0012 ms, 0.0430 ms

Status code distribution:
  [200] 500 responses

=== ec2 | concurrency=50 | 500 requests ===
Summary:
  Success rate: 100.00%
  Total:        12.8829 sec
  Slowest:      1.4869 sec
  Fastest:      0.0649 sec
  Average:      1.2300 sec
  Requests/sec: 38.8112

  Total data:   144.87 KiB
  Size/request: 296 B
  Size/sec:     11.24 KiB

Response time histogram:
  0.065 sec [1]   |
  0.207 sec [6]   |
  0.349 sec [3]   |
  0.491 sec [8]   |
  0.634 sec [5]   |
  0.776 sec [6]   |
  0.918 sec [5]   |
  1.060 sec [5]   |
  1.202 sec [45]  |■■■■
  1.345 sec [327] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  1.487 sec [89]  |■■■■■■■■

Response time distribution:
  10.00% in 1.1581 sec
  25.00% in 1.2299 sec
  50.00% in 1.2768 sec
  75.00% in 1.3288 sec
  90.00% in 1.3688 sec
  95.00% in 1.3899 sec
  99.00% in 1.4397 sec
  99.90% in 1.4869 sec
  99.99% in 1.4869 sec


Details (average, fastest, slowest):
  DNS+dialup:   0.0004 sec, 0.0002 sec, 0.0015 sec
  DNS-lookup:   0.0000 sec, 0.0000 sec, 0.0001 sec

Status code distribution:
  [200] 500 responses

=== Scenario B complete. Results in /home/ec2-user/lsc-agh-2026/loadtest/../results ===
```

```bash
$ bash loadtest/scenario-c.sh "$LAMBDA_ZIP_URL" "$LAMBDA_CONTAINER_URL" "$FARGATE_URL" "$EC2_URL"
=== Scenario C: Burst from Zero ===
Ensure Lambda has been idle for 20+ minutes.

NOTE: Lambda concurrency is capped at 10 (AWS Academy limit: max 10 concurrent
Lambda execution environments). Fargate/EC2 use c=50.

Launching burst to ALL targets simultaneously...

Summary:
  Success rate: 100.00%
  Total:        416.2554 ms
  Slowest:      113.4136 ms
  Fastest:      8.0673 ms
  Average:      20.2202 ms
  Requests/sec: 480.4743

  Total data:   13.28 KiB
  Size/request: 68 B
  Size/sec:     31.91 KiB

Response time histogram:
    8.067 ms [1]   |
   18.602 ms [157] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
   29.137 ms [31]  |■■■■■■
   39.671 ms [1]   |
   50.206 ms [0]   |
   60.740 ms [0]   |
   71.275 ms [0]   |
   81.810 ms [0]   |
   92.344 ms [0]   |
  102.879 ms [0]   |
  113.414 ms [10]  |■■

Response time distribution:
  10.00% in 10.1544 ms
  25.00% in 12.2203 ms
  50.00% in 16.0529 ms
  75.00% in 18.2651 ms
  90.00% in 21.0738 ms
  95.00% in 108.5183 ms
  99.00% in 112.4490 ms
  99.90% in 113.4136 ms
  99.99% in 113.4136 ms


Details (average, fastest, slowest):
  DNS+dialup:   88.8399 ms, 87.8030 ms, 89.7561 ms
  DNS-lookup:   0.0185 ms, 0.0124 ms, 0.0614 ms

Status code distribution:
  [403] 200 responses
Summary:
  Success rate: 100.00%
  Total:        464.5444 ms
  Slowest:      124.5887 ms
  Fastest:      7.6609 ms
  Average:      21.6607 ms
  Requests/sec: 430.5294

  Total data:   13.28 KiB
  Size/request: 68 B
  Size/sec:     28.59 KiB

Response time histogram:
    7.661 ms [1]   |
   19.354 ms [164] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
   31.046 ms [24]  |■■■■
   42.739 ms [1]   |
   54.432 ms [0]   |
   66.125 ms [0]   |
   77.818 ms [0]   |
   89.510 ms [0]   |
  101.203 ms [0]   |
  112.896 ms [0]   |
  124.589 ms [10]  |■

Response time distribution:
  10.00% in 10.9315 ms
  25.00% in 14.7431 ms
  50.00% in 16.4774 ms
  75.00% in 18.3670 ms
  90.00% in 22.4037 ms
  95.00% in 120.0388 ms
  99.00% in 124.5834 ms
  99.90% in 124.5887 ms
  99.99% in 124.5887 ms


Details (average, fastest, slowest):
  DNS+dialup:   95.8224 ms, 94.2608 ms, 97.3589 ms
  DNS-lookup:   0.0206 ms, 0.0128 ms, 0.0710 ms

Status code distribution:
  [403] 200 responses
Summary:
  Success rate: 100.00%
  Total:        5.3564 sec
  Slowest:      1.4974 sec
  Fastest:      0.1634 sec
  Average:      1.1971 sec
  Requests/sec: 37.3386

  Total data:   57.96 KiB
  Size/request: 296 B
  Size/sec:     10.82 KiB

Response time histogram:
  0.163 sec [1]   |
  0.297 sec [6]   |■
  0.430 sec [5]   |■
  0.564 sec [2]   |
  0.697 sec [9]   |■■
  0.830 sec [4]   |■
  0.964 sec [6]   |■
  1.097 sec [5]   |■
  1.231 sec [9]   |■■
  1.364 sec [106] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  1.497 sec [47]  |■■■■■■■■■■■■■■

Response time distribution:
  10.00% in 0.6251 sec
  25.00% in 1.2441 sec
  50.00% in 1.3075 sec
  75.00% in 1.3603 sec
  90.00% in 1.4106 sec
  95.00% in 1.4464 sec
  99.00% in 1.4844 sec
  99.90% in 1.4974 sec
  99.99% in 1.4974 sec


Details (average, fastest, slowest):
  DNS+dialup:   0.0006 sec, 0.0002 sec, 0.0023 sec
  DNS-lookup:   0.0000 sec, 0.0000 sec, 0.0000 sec

Status code distribution:
  [200] 200 responses
Summary:
  Success rate: 100.00%
  Total:        16.1547 sec
  Slowest:      4.5061 sec
  Fastest:      0.1388 sec
  Average:      3.6057 sec
  Requests/sec: 12.3803

  Total data:   61.30 KiB
  Size/request: 313 B
  Size/sec:     3.79 KiB

Response time histogram:
  0.139 sec [1]  |
  0.576 sec [5]  |■
  1.012 sec [3]  |■
  1.449 sec [7]  |■■
  1.886 sec [3]  |■
  2.322 sec [8]  |■■■
  2.759 sec [6]  |■■
  3.196 sec [5]  |■
  3.633 sec [3]  |■
  4.069 sec [75] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  4.506 sec [84] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■

Response time distribution:
  10.00% in 2.0152 sec
  25.00% in 3.7778 sec
  50.00% in 3.9966 sec
  75.00% in 4.1821 sec
  90.00% in 4.2906 sec
  95.00% in 4.3152 sec
  99.00% in 4.4931 sec
  99.90% in 4.5061 sec
  99.99% in 4.5061 sec


Details (average, fastest, slowest):
  DNS+dialup:   0.0019 sec, 0.0010 sec, 0.0029 sec
  DNS-lookup:   0.0000 sec, 0.0000 sec, 0.0001 sec

Status code distribution:
  [200] 200 responses

=== Scenario C complete. Results in /home/ec2-user/lsc-agh-2026/loadtest/../results ===
```