# Variáveis de Ambiente

Em uma aplicação real, é muito comum ter variáveis que podem mudar de acordo com o contexto que você está trabalhando. Essas variáveis podem ser relacionadas a configuração de banco de dados, até senhas e segredos utilizados para integrações com outras APIs.

É muito comum que esse tipo de dado não esteja no estilo "hard-coded" na aplicação, mas seja armazenada nas variáveis de ambiente da máquina que executará nossa aplicação. 

Com Kubernetes, não é diferente. Existem diferentes formas de termos esses arquivos de configuração e exploramos alguns deles no curso.

## Variáveis de Ambiente no Deployment

É possível configurar dentro do [Deployment](./kubernetes_deployment.md) as variáveis de ambiente que nossos [Pods](./kubernetes_pods.md) precisam.

Através do atributo `env` dentro de `spec.containers`, podemos definir objetos com duas propriedades:

- **name**: Nome da variável de ambiente
- **value**: Valor da variável de ambiente.

Exemplo de como ficaria isso dentro do deployment:

```yml
spec:
    containers:
        - env:
            - name: "SERVER_PORT"
              value: "3000"
```

Isso vai permitir com que em uma aplicação Node, por exemplo, eu faça isso:

```js
import express from 'express';

const app = express();

app.get('/', (req, res) => {
    return res.send('Hello World');
})

const PORT = process.env.SERVER_PORT;
app.listen(PORT, () => console.log(`Aplicação rodando na porta: ${PORT}`));
```

Onde o valor da variável `SERVER_PORT` dentro de `process.env`, vai ser igual ao valor que eu defini no [Deployment](./kubernetes_deployment.md)