Nas últimas aulas, precisamos criar e gerenciar Deployments e Pods, aprendemos a diferença entre Deployment e ReplicaSet e vimos que em um caso do mundo real, sempre usaremos Deployment para guardar o estado atual da aplicação que a gente quer rodar.

Porém, pode acontecer de alguma mudança no Deployment dar errado, e nesse caso, o que podemos fazer? O Kubernetes tem uma função que podem nos ajudar nisso:

## Rollout

Vimos que ao realizarmos uma mudança em um deployment, todos os pods são "terminados" e "recriados" com os containers atualizados. Porém, o ReplicaSet anterior a mudança continua existindo. Isso em primeiro momento pode parecer esquisito, mas é importante saber que é necessário que ele exista caso a gente precise fazer algum rollback de emergência, desfazendo o último deploy no cluster.

Pra isso, utilizamos o Rollout, que é uma função que permite "retroceder" ou "avançar" um Deployment de **Revisão**. Revisão é cada uma das versões utilizadas pelo Deployment, ou seja, se há uma atualização no Deployment, é gerada uma nova revisão.1

Esse comando é extremamente útil, pois garante ao Cluster a possibilidade de ser mitigado possíveis falhas no sistema decorrente de uma atualização no mesmo.