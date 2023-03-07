# Atividade sobre Docker e AWS
## Descrição da Atividade
-  instalação e configuração do DOCKER ou
CONTAINERD no host EC2;
Ponto adicional para o trabalho utilizar a
instalação via script de Start Instance
(user_data.sh)
- Efetuar Deploy de uma aplicação Wordpress
com:
container de aplicação
container database Mysql
- configuração da utilização do serviço EFS
AWS para estáticos do container de aplicação
Wordpress
- configuração do serviço de Load Balancer
AWS para a aplicação Wordpress

## Configuração da instância EC2

| TIPO DA INSTÂNCIA | AMI | ARMAZENAMENTO |
| --- | ----------- | ----------- |
| t3.small | AWS Linux | 8GiB (gp2) |


## Arquivo user_data.sh para configuração do Docker na inicialização da instância

```sh
#!/bin/bash
yum update -y
yum install httpd -y
yum install git -y
sudo amazon-linux-extras install docker
systemctl start docker
sudo usermod -a -G docker ec2-user
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```
OBS: Esse user_data.sh está fazendo a instalação do docker, apache, git, docker-compose e outras ferramentas necessarias para o funcionamento do docker.