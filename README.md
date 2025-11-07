Laboratório - 524 - CI/CD: Integração e entrega continua com Jenkins, Nexus, Sonarqube, Gitlab-ci e Github actions - LC
=============================

### Deploy no Kubernetes

Repositório para armazenar o Laboratório do curso de Integração e Entrega Continua com Git, Jenkins, Nexus e SonarQuebe da [4Linux][1]


Dependências
------------

Para a criação do laboratório é necessário ter pré instalado os seguintes softwares:

* [Git][2]
* [VirtualBox][3]
* [Vagrant][5]

> Para o máquinas com Linux aconselhamos, se possível, que as instalações sejam feitas pelo gerenciador de pacotes da distro utilizada. Se você quiser usar **KVM/QEMU/LIBVIRT** ao invés de **Virtualbox**, será necessário:

    1. Instale as dependências de compilação e de libvirt:

        sudo apt update
        sudo apt install -y \
        qemu-system libvirt-daemon-system libvirt-clients ebtables dnsmasq-base \
        ruby-dev libvirt-dev libxml2-dev libxslt-dev zlib1g-dev \
        build-essential make gcc pkg-config

    2. Verifique se o serviço do libvirt está rodando:

        sudo systemctl enable --now libvirtd
        sudo systemctl status libvirtd

    3. Instale o plugin do Vagrant:
        vagrant plugin install vagrant-libvirt

    4. Dê permissão ao seu usuário:
        sudo usermod -aG libvirt $(whoami)
        sudo usermod -aG kvm $(whoami)
        newgrp libvirt

    5. Suba o ambiente do Vagrant usando:
        vagrant up --provider=libvirt




> Para o máquinas com Windows aconselhamos, se possível, que as instalações sejam feitas pelo gerenciador de pacotes **[Cygwin][6]**.

> Para as máquinas com MAC OS aconselhamos, se possível, que as instalações sejam feitas pelo gerenciador de pacotes **brew**.

Laboratório
-----------

O Laboratório será criado utilizando o [Vagrant][7]. Ferramenta para criar e gerenciar ambientes virtualizados (baseado em Inúmeros providers) com foco em automação.

Nesse laboratórios, que está centralizado no arquivo [Vagrantfile][8], serão criadas 4 maquinas com as seguintes características:

Nome       | vCPUs | Memoria RAM | IP            | S.O.¹           | Script de Provisionamento²
---------- |:-----:|:-----------:|:-------------:|:---------------:| -----------------------------
cicd       | 1     | 1536MB      | 192.168.56.10 | ubuntu/focal64 |  provision/ansible/cicd.yaml |
cicd-tools | 1     | 3072MB      | 192.168.56.20 | dubuntu/focal64 | provision/ansible/cicd-tools |
k3s    | 1     | 2048MB       | 192.168.56.30 | ubuntu/focal64 | provision/ansible/k3s.yaml |
gitlab-ci | 1 | 3072 | 192.168.56.40 | ubuntu/focal64 | provision/ansible/gitlab-ci.yaml |

> **¹**: Esses Sistemas operacionais estão sendo utilizado no formato de Boxes, é a forma como o vagrant chama as imagens do sistema operacional utilizado.

> **²**: Para o Script de Provisionamento estamos utilizando Shell Script

Criação do Laboratório 
----------------------

Para criar o laboratório é necessário fazer o `git clone` desse repositório e, dentro da pasta baixada realizar a execução do `vagrant up`, conforme abaixo:

```bash
git clone -b k3s https://github.com/4linux/524
cd 524/
vagrant up
```

_O Laboratório **pode demorar**, dependendo da conexão de internet e poder computacional, para ficar totalmente preparado._

> **Atenção** Para máquinas físicas com apenas 8GB de RAM recomendamos ligar apenas duas máquinas por vez.

> Em caso de erro na criação das máquinas sempre valide se sua conexão está boa, os logs de erros na tela e, se necessário, o arquivo **/var/log/vagrant_provision.log** dentro da máquina que apresentou a falha.

Por fim, para melhor utilização, abaixo há alguns comandos básicos do vagrant para gerencia das máquinas virtuais.

Comandos                | Descrição
:----------------------:| ---------------------------------------
`vagrant init`          | Gera o VagrantFile
`vagrant box add <box>` | Baixar imagem do sistema
`vagrant box status`    | Verificar o status dos boxes criados
`vagrant up`            | Cria/Liga as VMs baseado no VagrantFile
`vagrant provision`     | Provisiona mudanças logicas nas VMs
`vagrant status`        | Verifica se VM estão ativas ou não.
`vagrant ssh <vm>`      | Acessa a VM
`vagrant ssh <vm> -c <comando>` | Executa comando via ssh
`vagrant reload <vm>`   | Reinicia a VM
`vagrant halt`          | Desliga as VMs

> Para maiores informações acesse a [Documentação do Vagrant][13]

[1]: https://4linux.com.br
[2]: https://git-scm.com/downloads
[3]: https://www.virtualbox.org/wiki/Downloads
[5]: https://www.vagrantup.com/downloads
[6]: https://cygwin.com/install.html
[7]: https://www.vagrantup.com/
[8]: ./Vagrantfile
[13]: https://www.vagrantup.com/docs
[14]: https://app.vagrantup.com/4linux
