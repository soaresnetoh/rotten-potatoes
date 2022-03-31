# rotten-potatoes

Fork da aplicação do [kubedev](https://github.com/KubeDev/rotten-potatoes) para desenvolvimento do aprendizado na Iniciativa Kubernetes que aconteceu em 03/2022.

Após a aula, foi possivel aprender a configurar o Kubernetes local com k3d e subir a aplicação no cluster.

## Configuração

variáveis de ambiente necessárias para congiruação do Mongo na aplicação:  

MONGODB_DB => Nome do database

MONGODB_HOST => Host do MongoDB

MONGODB_PORT => Posta de acesso ao MongoDB

MONGODB_USERNAME => Usuário do MongoDB

MONGODB_PASSWORD => Senha do MongoDB



# Para Testar

Voce pode usar o k3d, o kind e outras aplicações para rodar localmente um cluster k8s.

Aqui vou usar o k3d:

Instale seguindo o tutorial que está do site oficial [https://k3d.io/](https://k3d.io/)

Instale tambem o [kubectl](https://kubernetes.io/docs/tasks/tools/) e o [docker](https://docs.docker.com/engine/install/).

No nosso ambiente (uma maquina Core i5 com 16Gb ram) vamos subir 3 nós master e 3 nós agent, alem do Loadbalance.

```bash
k3d cluster create --agents 3 --servers 3 -p "8080:30000@loadbalancer"
```

Na pasta da aplicação,, siga os passos para criar a imagem do container e enviar para o [dockerhub](https://hub.docker.com/) (voce precisa ter uma conta no dockerhub -- ou outro registry -- para subir sua imagem)

```bash
cd src
docker build -t hernanisoares/rotten-potatoes:latest .
docker tag hernanisoares/rotten-potatoes:v1 hernanisoares/rotten-potatoes:latest
docker login
docker push hernanisoares/rotten-potatoes:v1
docker push hernanisoares/rotten-potatoes:latest
```

agora acesse a pasta onde estão os manifestos kubernetes e faça o deploy.


```bash
cd k8s
kubectl apply -f deployment.yaml
kubectl get all

```

acesse a aplicação em [http://localhost:8080](http://localhost:8080)








