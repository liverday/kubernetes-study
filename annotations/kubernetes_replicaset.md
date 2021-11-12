# ReplicaSet

É a configuração realizada com o propósito de sempre manter um número X de pods sendo executados por vez. É comumente utilizado para garantir disponibilidade em um cenário de falha. 

De maneira declarativa, podemos definir:

- Garantir que uma quantidade X de Pods estejam sempre sendo executados
- Um seletor que identifica qual pod poderá ser usado no critério de seleção de novos pods
- Um template utilizado no momento da criação de novos pods, com o objetivo de substituir um pod que falhou anteriormente.

É importante notar que uma vez criado o ReplicaSet, não é possível alterar os pods diretamente. Em caso de atualizações de imagem, é necessário recriar matar cada um dos pods do ReplicaSet, a fim dele criar novos pods com a imagem atualizada.

Para realizar essa atualização desejada de imagem, o segredo é utilizar [Deployments](#deployment) que implementam um ReplicaSet. Isso vai garantir que toda atualização de estado descrita pelo Deployment, impacte diretamente em todos os ReplicaSet e Pods abaixo dele.