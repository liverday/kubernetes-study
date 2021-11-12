# Pods

É um grupo de um ou mais containers representando uma aplicação no cluster Kubernetes. É a menor unidade de computação que é possível implantar em um Cluster. 

Os containers de um Pod são sempre localizados no mesmo lugar, e compartilhando a mesma estratégia de agendamento, sendo executada em um contexto compartilhado. Ou seja, no mesmo [Workload](./kubernetes_components.md#workload). 