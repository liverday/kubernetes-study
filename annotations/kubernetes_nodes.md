# Nodes

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