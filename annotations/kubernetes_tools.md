Kubernetes não é somente uma biblioteca, ou um utilitário. É um ecossistema inteiro que envolve muitas frentes presentes na cultura DevOps e é preciso entender isso.

Dentro dos diferentes cenários e ambientes de desenvolvimento, temos diferentes ferramentas para otimizar o trabalho com Kubernetes. E nesse arquivo, quero listar algumas das ferramentas mais utilizadas e explicar o motivo de uso delas:

## Kubectl

É a CLI oficial do Kubernetes para gerenciar os nossos clusters. Utiliza o arquivo `$HOME/.kube/config` para configurá-los e todas as operações executadas no Kubectl alterarão esse arquivo diretamente.

## Kind

Permite com que você possa criar um cluster Kubernetes localmente. É necessário ter o Docker instalado e configurado em sua máquina.

## Minikube

Muito parecido com o [Kind](#kind), permite executar um cluster Kubernetes localmente. Diferente do Kind, é possível rodar o cluster Kubernetes em uma máquina virtual, um container Docker, ou em [Bare metal](https://under.com.br/o-que-e-bare-metal/)

