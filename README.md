```
########## How To Build Docker Image #############
##
##  Build image from Dockerfile. docker build -t denny/mytest:v1 --rm=true .
##  Run docker intermediate container:
##         docker run -t -d --privileged -h mytest --name my-test -p 5122:22 denny/mytest:v1 /usr/sbin/sshd -D
##         docker run -t -i --privileged -h mytest --name my-test denny/mytest:v1 /bin/bash
##
##  Commit local image:
##    docker commit -m "Initial version" -a "Denny Zhang<denny@dennyzhang.com>" e955748a2634 denny/osc:v1
##    # Get docker user credential first
##    docker login
##    docker push denny/mytest:v1
##    docker history denny/mytest:v1
##
##################################################
```

Test docker image with chef
```
echo "cookbook_path '/root/iamdevops/cookbooks'" > /root/client.rb
echo "{\"run_list\": [\"recipe[jenkins-auth::conf_jenkins]\"], \"os_basic\":{\"enable_firewall\":\"0\"}, \"jenkins_auth\":{\"jobs\":\"BuildRepoCode,UpdateSandbox,UpdateJenkinsItself\"}}" > /root/client.json

echo "{\"run_list\": [\"recipe[autotest-auth::default]\"]}" > /root/client.json
cd /root/test/master/iamdevops/cookbooks
git pull

chef-solo --config /root/client.rb -j /root/client.json
```