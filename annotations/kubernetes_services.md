# Services

É uma maneira abstrata de expor um set de [Pods](./kubernetes_pods.md) (Deployment, Replicaset, Workload) como um serviço que pode ser acessado via rede. Com Kubernetes não é necessário ter um Service Discovery externo, ele tem um mecanismo de dar um endereço de IP e um DNS próprio para cada um dos Pods do set, e faz um "balanceamento de carga" entre eles.

Se fossemos resumir sua responsabilidade, é ser a porta de entrada para cada um dos Pods que você deseja expor para fora do Cluster.

Para realizar esse tipo de tarefa, o Kubernetes possui três tipos de serviços:

## ClusterIP

Responsável por expor o serviço desejado em um IP interno do cluster. Essa opção torna o serviço disponível apenas para objetos que existem dentro do Cluster. Sendo necessário um `port-forward` para expor para fora. É o tipo de serviço padrão do Kubernetes.

## NodePort

Cria uma porta estática em cada um dos Nodes e atrela isso à um serviço. Fazendo com que qualquer requisição utilizando `<Node IP>:<Node Port>` seja redirecionada ao serviço desejado.

Por trás dos panos, um serviço [ClusterIP](#clusterip) é criado para realizar o roteamento do Node para o Serviço.

Esse tipo de serviço tem uma característica: Somente portas no range de `30.000-32.767` são aceitas em sua definição. 

## LoadBalancer

Expõe o serviço externamente utilizando um Load Balancer de algum Cloud Provider. Serviços [ClusterIP](#clusterip) e [NodePort](#nodeport) para cada uma das rotas do LoadBalancer externo são criados automáticamente.

## ExternalName

Mapeia o serviço para o conteúdo do campo `externalName` presente em seus metadados. Esse campo é responsável por armazenar o DNS que será utilizado para identificar o serviço no Cluster.  