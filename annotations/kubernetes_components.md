O kubernetes é composto por diferentes componentes que permitem com que ele seja um orquestrador completo. Vamos explorar alguns deles.

## Kubectl

É a CLI do Kubernetes, que permite com que o usuário interaja com o [kube-apiserver](#kube-apiserver) utilizando linhas de comando.

## Workload

É a representação de uma aplicação rodando no Kubernetes. Sendo sua aplicação um único componente, ou o agrupamento de vários deles, através da criação e gerenciamento de [Pods](./kubernetes_pods.md).

## Control Plane

Esse componente é responsável por gerenciar os [Nodes](./kubernetes_nodes.md) no cluster Kubernetes, sendo responsável por, através de seus componentes, permitir com que possamos tomar decisões globais a respeito dos workers do mesmo, como: 

- Agendamento de Pods
- Detecção de falhas e recriação de Pods instáveis
- Replicar Deployments para permitir maior tolerância a falhas.

Para conseguir entender o que ele é por completo, é preciso entender os seus componentes

### kube-apiserver

O API Server é o componente responsável por expor a API do Kubernetes. Expondo uma HTTP API, sendo possível ser consumida utilizando gRPC ou REST, é uma API que permite com que um cliente final consiga obter e manipular o estado de todos os objetos do Kubernetes, como: Pods, Namespaces, ConfigMaps e Eventos)

kube-apiserver é a implementação principal da API do Kubernetes. Desenhada para escalar horizontalmente, é fácilmente escalável através da criação de mais instâncias e balanceamento de carga entre elas.

### etcd

É um store distribuido que armazena dados do tipo `key=value` usado para armazenar os dados de todo o cluster Kubernetes.

### kube-scheduler

É o componente responsável por observar a criação de novos Pods que não estão em nenhum Node, e selecionar um Node para executá-los.

Permitindo com que muito fatores impactem em sua decisão, como:

- Recursos necessários para execução individual ou coletiva
- Regras de Software/Hardware/Privacidade
- Afinidade
- Localidade do dado
- Etc

### kube-controller-manager

Componente que executa processos de controle. Esses métods são loops que observam a mudança de um estado compartilhado pela [kube-apiserver](#kube-apiserver) e realiza mudanças tentando passar de um estado atual, para o estado desejado, como por exemplo, após a criação de um deployment que descreve a criação um Pod de [Nginx](https://nginx.com), realizar a criação do mesmo no workload desejado.

Cada processo de controle é compilado e executado separadamente, porém, para reduzir complexidade, utilizam o mesmo arquivo binário e é executado em um único lugar.

Existem diferentes tipo de processos de controle (controllers):

- Node Controllers: responsável por notificar as partes interessadas quando um Node para de funcionar.
- Job Controller: Observa tarefas que representam uma única execução, então cria os Pods para executá-la e terminá-la.
- Endpoints Controller: Popula os endpoints de um determinado workload.
- Service Account e Token Controllers: Cria contas e tokens para [Namespaces](#namespaces) recém-criadas.

### cloud-controller-manager

Muito parecido com o [kube-controller-manager](#kube-controller-manager), esse componente é responsável por manter e executar processos de controle. Porém, com contexto específico para a integração com Cloud Providers, como: GCP, AWS e Azure.