---
layout: "post"
title: "gs-codedeploy"
date: "2017-02-01 16:56"
---

http://www.daemon.pe.kr/post/1086
https://aws.amazon.com/ko/blogs/devops/using-codedeploy-environment-variables/
http://blog.powerupcloud.com/2016/03/24/deployment-automation-using-aws-code-depoly/
http://www.cuelogic.com/blog/how-to-use-jenkins-and-aws-code-deploy-as-a-ci-cd-tool/
http://www.allcloud.io/how-to/mastering-aws-codedeploy-with-jenkins-and-puppet/
http://www.daemon.pe.kr/post/1086
http://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/app-spec-ref-hooks.html
http://blog.gsclip.com/2015/09/codedeploy-%EC%9D%B4%ED%95%B4/
http://sarc.io/index.php/aws/564-cloudformation-codedeploy
http://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/how-to-add-appspec-file.html
http://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/app-spec-ref-permissions.html
https://www.google.co.kr/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=appspec.yml+files+permission

## 왜 CodeDeploy를 사용해야 하는가?

## 젠킨스에 코드디플로이 설치
The AWS CodeDeploy Jenkins plugin provides a post-build step for your Jenkins project. Upon a successful build, it will zip the workspace, upload to S3, and
start a new deployment. Optionally, you can set it to wait for the deployment to finish, making the final success contingent on the success of the deployment.

### Setting up
After building and installing the plugin, some simple configuration is needed for your project.

- Open up your project configuration
- In the Post-build Actions section, select Deploy an application to AWS CodeDeploy
- Application Name, Deployment Group, Deployment Config, and region are all required options.
- For authentication, there are two options. Either option requires that the associated role has, at minimum, a policy that permits codedeploy:* and s3:Put*.
  - Access/Secret key pair. For example, the keys associated with a specific IAM user. If left blank, the default chain of credentials will be checked.
  - Temporary access keys. These will use the global keys from the Jenkins instance.


AWS CodeDeploy 젠킨스 플러그인은 젠킨스 프로젝트의 포스트 빌드 후 단계를 제공합니다. 빌드가 성공적으로 완료되면 작업 공간을 압축하고 S3에 업로드 한 다음 새 배치를 시작합니다. 선택적으로 배포가 완료될 때까지 기다리도록 설정하여 배포의 성공 여부에 따라 최종 결과를 얻을 수 있습니다.

## 권한
코드디플로이에는 세 가지 롤이 필요합니다.
- 코드디플로이를 사용하는 클라이언트(보통 젠킨스)
- 코드디플로이가 서비스를 배포하는 인스턴스
- 코드디플로이

각 롤마다 필요한 정책은 다음과 같습니다.

### 젠킨스
https://wiki.jenkins-ci.org/display/JENKINS/AWS+Codedeploy+plugin
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codedeploy:*",
         "s3:*"
      ],
      "Resource": "*"
    }
  ]
}
```
![젠킨스 플러그인 설명]()

### 인스턴스
CodeDeploy를 사용하기 위해서 인스턴스는 S3에 있는 리소스를 읽을 수 있어야 합니다. 기존에 디폴트로 생성된 정책명은 `AmazonS3ReadOnlyAccess` 이며 정책을 자세히 보면 아래와 같습니다.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "*"
    }
  ]
}
```
### 코드디플로이
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:CompleteLifecycleAction",
        "autoscaling:DeleteLifecycleHook",
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeLifecycleHooks",
        "autoscaling:PutLifecycleHook",
        "autoscaling:RecordLifecycleActionHeartbeat",
        "autoscaling:CreateAutoScalingGroup",
        "autoscaling:UpdateAutoScalingGroup",
        "autoscaling:EnableMetricsCollection",
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribePolicies",
        "autoscaling:DescribeScheduledActions",
        "autoscaling:DescribeNotificationConfigurations",
        "autoscaling:DescribeLifecycleHooks",
        "autoscaling:SuspendProcesses",
        "autoscaling:ResumeProcesses",
        "autoscaling:AttachLoadBalancers",
        "autoscaling:PutScalingPolicy",
        "autoscaling:PutScheduledUpdateGroupAction",
        "autoscaling:PutNotificationConfiguration",
        "autoscaling:PutLifecycleHook",
        "autoscaling:DescribeScalingActivities",
        "autoscaling:DeleteAutoScalingGroup",
        "ec2:DescribeInstances",
        "ec2:DescribeInstanceStatus",
        "ec2:TerminateInstances",
        "tag:GetTags",
        "tag:GetResources",
        "sns:Publish",
        "cloudwatch:DescribeAlarms",
        "elasticloadbalancing:DescribeLoadBalancers",
        "elasticloadbalancing:DescribeInstanceHealth",
        "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
        "elasticloadbalancing:DeregisterInstancesFromLoadBalancer"
      ],
      "Resource": "*"
    }
  ]
}
```

마지막으로 인스턴스 생성시 롤을 지정할 수 있는 권한이 필요합니다.
### 설정
- 프로젝트 구성 열기
- 빌드 후 작업 섹션에서 AWS CodeDeploy에 응용 프로그램 배포를 선택합니다.
- 응용 프로그램 이름, 배포 그룹, 배포 구성 및 리전은 모두 필수 옵션입니다.
- 인증에는 두 가지 옵션이 있습니다. 두 옵션 모두 관련 역할에 최소한 `codedeploy:*` 와 `s3:Put*` 을 허용하는 정책이 있어야합니다.
   - 액세스 / 비밀 키 쌍. 예를 들어, 특정 IAM 사용자와 연관된 키. 공란으로두면 기본 자격 증명 체인이 선택됩니다.
   - 임시 액세스 키. Jenkins 인스턴스의 전역 키를 사용합니다.


## 인스턴스 설정
인스턴스를 생성하고 나서는 role를 등록하거나 변경할 수 없습니다. 따라서 인스턴스 생성시 서비스별로 role를 생성하고 여기에 서비스에 맞는 정책을 관리하는 것이 바람직해보입니다.

CodeDeploy를 사용하기 위해서 인스턴스는 S3에 있는 리소스를 읽을 수 있어야 합니다. 기존에 디폴트로 생성된 정책명은 `AmazonS3ReadOnlyAccess` 이며 정책을 자세히 보면 아래와 같습니다.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

또한 인스턴스에는 CodeDeploy Agent 가 설치되어 있어야 합니다. 인스턴스를 생성하고 설치해도 좋지만 인스턴스가 생성되는 시점에 Agent를 설치하도록 user data에 아래 스크립트를 입력합니다.

```sh
#!/bin/bash
sudo yum update && sudo yum install ruby wget
cd /home/ec2-user
wget https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto

# 노드 환경 변수 설정
BASHRCFILE=~/.bashrc
NODE_ENV=production-bate
echo "export NODE_ENV=${NODE_ENV}" >> ${BASHRCFILE}
echo "NODE_ENV=production-bate" >> /etc/environment
vim /etc/environment
```

인스턴스가 생성이 완료되면 터미널에 접속하여 에이전트가 제대로 설치되었는지 확인합니다.

```sh
$ sudo service codedeploy-agent status

The AWS CodeDeploy agent is running as PID 2588
```

야호! 정상적으로 설치되어 동작하고 있습니다! 이제 본격적으로 CodeDeploy 설정을 시작해봅시다.

## CodeDeploy 설정
CodeDeploy 서비스도 동작하기 위한 role이 필요합니다. 디폴트로 생성된 정책명은 AWSCodeDeployRole 이며 자세한 정책은 아래와 같습니다.

- Application name: 애플리케이션의 이름을 지정합니다.
- Deployment group name: 배포 그룹을 지정합니다. 여기서 배포 그룹이란 인스턴스들의 모임을 말합니다. 하나의 애플리케이션에는 여러개의 배포그룹을 지정할 수 있습니다. 개발/운영 형식으로 배포 그룹을 지정하도록 한 것 같은데 우리는 아예 계정이 다르므로 이렇게 사용할 수 있을지...

### Environment configuration
Manually provision instances를 선택하고 배포되길 원하는 태그의 키와 값을 입력합니다. 이전에 우리가 생성한 인스턴스가 선택되도록 알아서 잘 적습니다.

### Deployment configuration
- CodeDeployDefault.OneAtATime
- CodeDeployDefault.AllAtOnce
- CodeDeployDefault.HalfAtATime


### 배포 타입
- in-place deployment : 배포 그룹의 인스턴스를 최신 응용 프로그램 버전으로 업데이트합니다. 배포하는 동안 각 인스턴스는 업데이트를 위해 잠시 오프라인 상태가 됩니다.
- blue/green deployment : 배포 그룹의 인스턴스를 새 인스턴스로 바꾸고 여기에 새로운 최신 응용 프로그램의 버전을 배포합니다. 새로 생성된 인스턴스가 로드 밸런서에 등록 된 후에 기존 환경에 인스턴스는 로드 밸런서에서 빠지고 종료됩니다.

입맛에 맞게 선택하면 됩니다. 단 한 순간도 오프라인 상태가 되면 안된다면 블루/그린 배포를 선택하세요.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:CompleteLifecycleAction",
        "autoscaling:DeleteLifecycleHook",
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeLifecycleHooks",
        "autoscaling:PutLifecycleHook",
        "autoscaling:RecordLifecycleActionHeartbeat",
        "autoscaling:CreateAutoScalingGroup",
        "autoscaling:UpdateAutoScalingGroup",
        "autoscaling:EnableMetricsCollection",
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribePolicies",
        "autoscaling:DescribeScheduledActions",
        "autoscaling:DescribeNotificationConfigurations",
        "autoscaling:DescribeLifecycleHooks",
        "autoscaling:SuspendProcesses",
        "autoscaling:ResumeProcesses",
        "autoscaling:AttachLoadBalancers",
        "autoscaling:PutScalingPolicy",
        "autoscaling:PutScheduledUpdateGroupAction",
        "autoscaling:PutNotificationConfiguration",
        "autoscaling:PutLifecycleHook",
        "autoscaling:DescribeScalingActivities",
        "autoscaling:DeleteAutoScalingGroup",
        "ec2:DescribeInstances",
        "ec2:DescribeInstanceStatus",
        "ec2:TerminateInstances",
        "tag:GetTags",
        "tag:GetResources",
        "sns:Publish",
        "cloudwatch:DescribeAlarms",
        "elasticloadbalancing:DescribeLoadBalancers",
        "elasticloadbalancing:DescribeInstanceHealth",
        "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
        "elasticloadbalancing:DeregisterInstancesFromLoadBalancer"
      ],
      "Resource": "*"
    }
  ]
}
```
참 많은 권한을 필요로 하네요.

ongratulations! The application recruit-backoffice has been created.
Before you deploy your application, you need to upload its files to a repository. You can use the AWS CLI to upload to an S3 bucket. (Want to use a different repository type? Learn more )
Create An AWS CodeDeploy AppSpec File. This File Defines How An Application Is Deployed To A Single Instance. Learn more
Download and install the AWS CLI, and then use it to upload your application to your preferred storage location. Learn more
Push and deploy your application. Use the AWS CLI to access the directory that contains your application files, and then run the following command:
aws deploy push --application-name <MyAppName> \
      --s3-location s3://<MyBucketName>/<MyNewAppBundleName> \
      --source <PathToMyBundle>
After you run this command, the AWS CLI will return instructions for deploying the bundle by calling "aws deploy create-deployment."



Deploy New revision 생성
Actions > Deploy new revision 클릭
Application : CodeDeploy 설정할때 만들었던 Application Name
Deployment Group : Application 설정된 Group 리스트가 보인다.
Revision Location : S3://생성한 bucket-name/파일위치/파일명(zip, tar, tar.gz)
Deployment Config : CodeDeployDefalut.OneAtTime(일반적인 설정 환경에 맞게 선택)
Deploy Now 클릭


## 로컬에서 배포해보기
```sh
aws deploy push --application-name recruit-backoffice \
      --s3-location s3://hwangdezang/SampleApp.zip \
      --source build

###
```
```
$ aws deploy push --application-name recruit-backoffice \
--s3-location s3://recruit/recruit-backoffice.zip \
--source build

Failed to upload 'build' to 's3://recruit/recruit-backoffice.zip': An error occurred (AllAccessDisabled) when calling the CreateMultipartUpload operation: All access to this object has been disabled
```

```sh
aws deploy create-deployment --application-name recruit-backoffice --s3-location bucket=hwangdezang,key=SampleApp.zip,bundleType=zip,eTag=0c85b90b758ef60d682dbe9a2849bbe6-2 --deployment-group-name <deployment-group-name> --deployment-config-name <deployment-config-name> --description <description>
```

```sh
aws deploy create-deployment --application-name CodeDeployTest --deployment-group-name CodeDeployTestGroup --s3-location bucket=hwangdezang,key=SampleApp.zip,bundleType=zip

###

aws deploy create-deployment --application-name recruit-backoffice --deployment-group-name playground --s3-location bucket=hwangdezang,key=SampleApp.zip,bundleType=zip
```

## appspec.yml 파일
ApplicationStop
DownloadBundle²
BeforeInstall
Install²
AfterInstall
ApplicationStart
²는 AWS CodeDeploy 작업을 위해 예약되어 있습니다. 스크립트를 실행하는 데 사용할 수 없습니다.


Requirements for each instance in the deployment:
Each Amazon EC2 instance must be launched with the correct IAM instance profile attached. Learn more
Each Amazon EC2 instance must have identifying Amazon EC2 tags (Learn more ) or be in an Auto Scaling group. Learn more
Each on-premises instance must have an associated IAM user, identifying on-premises instance tags, and a configuration file. Learn more
The AWS CodeDeploy agent must be installed and running on each instance. Learn more

aws deploy push --application-name <MyAppName> \
      --s3-location s3://<MyBucketName>/<MyNewAppBundleName> \
      --source <PathToMyBundle>

----

#### 참고
- http://www.slideshare.net/awskorea/aws-code-services-special-amazon-dev-ops-codedeploy-codepipeline-voiceops-chatops
- http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-run-agent-install.html
- http://cloud.hosting.kr/amazon-codepipeline-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-codedeploy-s3/
