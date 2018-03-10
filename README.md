# AnsibleInsideDocker
Ansible inside a Docker and configure other containers

Docker image build commands:

docker build -t apache-php --no-cache=true ./apache
docker build -t nginx --no-cache=true ./nginx
docker build -t ansible --no-cache=true ./ansible

Commands to build container:

docker run -d --name testapachephp2 -p 80:80 -v /home/ubuntu/nginx/samplekey.pub:/root/.ssh/authorized_keys apache-php /bin/bash
sudo docker run -d --name testnginx -p 8080:80 -v /home/ubuntu/nginx/samplekey.pub:/root/.ssh/authorized_keys nginx
sudo docker run --privileged -d \
    --link testnginx \
    --volume=/home/ubuntu/nginx/samplekey:/root/.ssh/samplekey \
    --volume=/home/ubuntu/nginx/samplekey.pub:/root/.ssh/samplekey.pub \
    --volume="$(pwd)/files":/etc/ansible/roles/role_under_test:ro \
	--name ansible-test \
    centos7-ansible /sbin/init
    
[ Before Creating the containers, create ssh key in host machine and use that key in ansible. Use the same as authorized key in other containers ]

