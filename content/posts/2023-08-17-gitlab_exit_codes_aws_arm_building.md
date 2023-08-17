---
title: "gitlab exit codes aws arm building"
date: 2023-08-17
draft: false
categories: [ daily ]
tags : [ blog]
---
# gitlab and exit codes
Sometimes gitlab will have a command or a script that runs and it will not trap an exit code properly.  
This results in an error condition happening in a script, but the job in the pipeline to come back green.   
here is some meat and context around the issue as well.   [https://gitlab.com/gitlab-org/gitlab-runner/-/issues/25394](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/25394)  and then a bit more here [https://gitlab.com/gitlab-org/gitlab/-/merge_requests/77601](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/77601)  and then of course it still isn't fixed  as of today's writing [https://gitlab.com/gitlab-org/gitlab/-/issues/383355](https://gitlab.com/gitlab-org/gitlab/-/issues/383355)
I have no idea exactly why this happens but this is one way that you can trap the exit code and then emit it for a job.  i've done it this way and it works at least as of 07012023   so who knows your mileage may vary 
```
make_thing:
  stage: something

  script:
    - set +e
    #making apt quieter https://peteris.rocks/blog/quiet-and-unattended-installation-with-apt-get/
    - export DEBIAN_FRONTEND=noninteractive
    - apt-get update -qq && apt-get install -y -qq --no-install-recommends apt-utils
    - apt-get install -qq -y -o Dpkg::Use-Pty=0 gcc libglib2.0-0 libsm6 libxrender1 libxext6 unzip curl zip
    - curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    - unzip -qq awscliv2.zip
    - ./scripts/i_do_a_thing.sh || EXIT_CODE=$?
    - exit $EXIT_CODE
```

# aws arm and amd  

there are times that the gitlab-runner is going to toss an error during the docker build.  
it will look like this:
failed to get destination image "sha256:2a7a208f3483cb09eb7afdfda6aca1f25b1ce5ccce8daae830492d53a79a983b": image with reference sha256:2a7a208f3483cb09eb7afdfda6aca1f25b1ce5ccce8daae830492d53a79a983b was found but does not match the specified platform: wanted linux/amd64, actual: linux/arm64/v8
this is curious and kind of obvious when you see it
```
docker build --platform "linux/amd64" -t $IMAGE_URL:$IMAGE_TAG .
echo Pushing the Docker image...
docker push $IMAGE_URL:$IMAGE_TAG
```
if the gitlab runner is on an AWS runner that is intel based and NOT AMD the error does not manifest cause ¯\_(ツ)_/¯ 

The solution is to remove the amd arch targeting 
```
docker build -t $IMAGE_URL:$IMAGE_TAG .
```
the arch targeting was put in place as a cover your butt tactic because if you were to build on a Mac for apple silicon then you are building for ARM and if the ARM image is not run on arm hardware then things blow up.  