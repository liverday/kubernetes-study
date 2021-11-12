# Deployment

Um deployment é responsável por prover de maneira declarativa, atualizações para [Pods](#pod) e [ReplicaSet](#replica-set).

Foi meio difícil de entender isso, mas é a representação de um "desejo" de mudança. Ou seja, sempre que eu precisar realizar algum tipo de interação com o meu Cluster de forma declarativa, eu uso um deployment para isso.

Parece confuso, mas é mais simples do que parece. Imagine o cenário de que precisaremos subir um servidor [nginx](https://nginx.com) no nosso workload [Workload](#workload). Nos criamos um deployment que descreve como essa mudança de estado será realizada. Com o deployment, é possível dizer quantos Pods precisarão ser criados, quais dados serao utilizados para identificá-los no cluster e etc.

É possível ver mais definições do Deployment [aqui](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)