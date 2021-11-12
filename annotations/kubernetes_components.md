O kubernetes é composto por diferentes componentes que permitem com que ele seja um orquestrador completo. Vamos explorar alguns deles.

## Kubectl

É a CLI do Kubernetes, que permite com que o usuário interaja com o [kube-apiserver](#kube-apiserver) utilizando linhas de comando.

## Workload


É a representação de uma aplicação rodando no Kubernetes. Sendo sua aplicação um único componente, ou o agrupamento de vários deles, através da criação e gerenciamento de [Pods](#pod)
Para facilitar a vida do desenvolvedor que precisará gerenciar os diversos [Pods](#pod) de um Workload, existem diferentes recursos que facilitam esse processo:

### Deployment

Um deployment é responsável por prover de maneira declarativa, atualizações para [Pods](#pod) e [ReplicaSet](#replica-set).

Foi meio difícil de entender isso, mas é a representação de um "desejo" de mudança. Ou seja, sempre que eu precisar realizar algum tipo de interação com o meu Cluster de forma declarativa, eu uso um deployment para isso.

Parece confuso, mas é mais simples do que parece. Imagine o cenário de que precisaremos subir um servidor [nginx](https://nginx.com) no nosso workload [Workload](#workload). Nos criamos um deployment que descreve como essa mudança de estado será realizada. Com o deployment, é possível dizer quantos Pods precisarão ser criados, quais dados serao utilizados para identificá-los no cluster e etc.

É possível ver mais definições do Deployment [aqui](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

### ReplicaSet

É a configuração realizada com o propósito de sempre manter um número X de pods sendo executados por vez. É comumente utilizado para garantir disponibilidade em um cenário de falha. 

De maneira declarativa, podemos definir:

- Garantir que uma quantidade X de Pods estejam sempre sendo executados
- Um seletor que identifica qual pod poderá ser usado no critério de seleção de novos pods
- Um template utilizado no momento da criação de novos pods, com o objetivo de substituir um pod que falhou anteriormente.

É importante notar que uma vez criado o ReplicaSet, não é possível alterar os pods diretamente. Em caso de atualizações de imagem, é necessário recriar matar cada um dos pods do ReplicaSet, a fim dele criar novos pods com a imagem atualizada.

Para realizar essa atualização desejada de imagem, o segredo é utilizar [Deployments](#deployment) que implementam um ReplicaSet. Isso vai garantir que toda atualização de estado descrita pelo Deployment, impacte diretamente em todos os ReplicaSet e Pods abaixo dele.

## Pod

É um grupo de um ou mais containers representando uma aplicação no cluster Kubernetes. É a menor unidade de computação que é possível implantar em um Cluster. 

Os containers de um Pod são sempre localizados no mesmo lugar, e compartilhando a mesma estratégia de agendamento, sendo executada em um contexto compartilhado. Ou seja, no mesmo [Workload](#workload). 

## Nodes

Nodes são as máquinas que serão responsáveis por executar [Workloads](#workload), sendo responsável por armazenar o estado de cada um de seus [Pods](#pod).

Nodes podem ser máquinas virtuais ou físicas, depende muito da forma como o Cluster foi configurado.

Cada node possui, pelo menos, três componentes Core do Kubernetes:

### kubelet

Um agent que é executado em cada Node de um determinado cluster. É ele quem garante que um container esteja sendo executado por um [Pod](#pod).

Recebe um arquivo de configuração que através de vários mecanismos, garante que os containers declarados no arquivo esteja executando e esteja sadío. O kubelet não gerencia containers que não foram criados em um cluster Kubernetes.

### kube-proxy

É um proxy de rede que é executado em cada Node de um determinado cluster. Através desse componente é possível criar regras de rede permitindo comunicação dentro e fora do cluster.

### Container Runtime

Cada Node é responsável por ter um Software que permite a execução de containers.

Um Node Kubernetes suporta diversas ferramentas, como: [Docker](https://docs.docker.com/engine/), [containerd](https://containerd.io/docs/), [cri-o](https://cri-o.io/#what-is-cri-o) e qualquer outra implementação da [Kubernetes CRI](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md) 

## Control Plane

Esse componente é responsável por gerenciar os [Nodes](#nodes) no cluster Kubernetes, sendo responsável por, através de seus componentes, permitir com que possamos tomar decisões globais a respeito dos workers do mesmo, como: 

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