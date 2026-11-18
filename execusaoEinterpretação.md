**executando aplicativo em docker**

"
root@controlplane:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
root@controlplane:~$ 
"
. aqui pedimos um diagnostico ao docker que mostra que não a nada em execução

"
root@controlplane:~$ docker run -d --name comunicado -p 8080:80 nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
881fc932939e: Pull complete 
77a3bfea78f5: Pull complete 
04c3905a997c: Pull complete 
7cb374baf968: Pull complete 
4c8c172c6586: Pull complete 
1dc588c89da1: Pull complete 
61d8f4c85a74: Pull complete 
c4f3d97811f6: Pull complete 
Digest: sha256:0563c36f94a6d3ec3584161b77859f740f7e20b4853f52c796a864ce6908e8ea
Status: Downloaded newer image for nginx:alpine
8a10329e7a95d49866465979e003fc30234c567c6800d8e49ecda4ab545347ee
root@controlplane:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
8a10329e7a95   nginx:alpine   "/docker-entrypoint.…"   12 seconds ago   Up 11 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   comunicado
root@controlplane:~$ 
"
. aqui temos o download do docker alpine nginx chamado comunicado

"
root@controlplane:~$ curl http://localhost:8080
<html>
<p> Gleidson Alves Vieira </p>
</html>
root@controlplane:~$
"
. e aqui temos um curl simples mostrando o resultado da saido apos o push de um html simples com meu nome dentro

**aplicação em kubernetes**
"
root@controlplane:~$ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
comunicado-57658f54ff-ctxbb   1/1     Running   0          114s
root@controlplane:~$ 
"
. aqui temos o comando pedido comprovando um funcionamento de um pod

"
root@controlplane:~$ kubectl scale deployment comunicado --replicas=3
deployment.apps/comunicado scaled
root@controlplane:~$ kubectl get pod 
NAME                     READY   STATUS    RESTARTS   AGE
comunicado-57658f54ff-95b7l   1/1     Running   0          14s
comunicado-57658f54ff-fpr29   1/1     Running   0          10m
comunicado-57658f54ff-m4zgc   1/1     Running   0          15m
root@controlplane:~$ kubectl delete pod $(kubectl get pods -o jsonpath='{.items[0].metadata.name}')
pod "comunicado-57658f54ff-95b7l" deleted from default namespace
root@controlplane:~$ kubectl get pod
NAME                     READY   STATUS    RESTARTS   AGE
comunicado-57658f54ff-fpr29   1/1     Running   0          10m
comunicado-57658f54ff-fwws2   1/1     Running   0          5s
comunicado-57658f54ff-m4zgc   1/1     Running   0          15m
root@controlplane:~$
"
. aqui temos o aumento do deply seguido da exclusão e comprovação de sucesso

"
root@controlplane:~$ kubectl run teste --image=curlimages/curl --rm -it --restart=never --curl hhtp:comunidaco-service
error: unknown flag: --curl
"
. aqui deixo a evidencia apenas de um erro, acredito ter me perdido nessa area e não me lembrar de como eu deveria executar isso no momento sigo então para a próxima seção

**interpretação dos resultados**

questão 1
uma imagem docker e como uma planta baixa instruindo ao computador como construir um docker enquanto o conteiner em execução é essa imagem ja construida e pronta para ser trabalhada

questão 2
ele serve para que se mantenha basicamente um serviço sendo executado, gerenciando se a um docker esta ativo e como no caso do escalamento que fizemos mantendo varios conteiners ativos para que possam ser acessados e restituindo os caso parem de funcionar

questão 3
um novo pod foi criado automaticamente em seu lugar ja que escalamos o deploymente para 3 ele assim manteve mesmo depois de um deles ser excluido manualmente

questão 4
na pratica um pod estaria rodando por exemplo uma pagina web e não simplesmente um arquivo solto de teste, para que possa ser acessado por seja la quem tem acesso a conexão com esse conteiner, dessa forma usuarios reais podem acessar esses serviços e caso caiam um novo serviço e criado para que possa ser acessado novamente

7 conclusão
durante as Aulas praticas revisitei temas e areas em que ja havia estudado antes, a criação e execução de conteiners ja era uma area de interrese pessoal, ainda assim esse conhecimento raso foi aprofundado grandemente pelo uso e acesso do serviço de kubernets. nas aulas praticas pude experienciar o basicamente o processo de aplicação e manutenção do serviço que mantem os sites no ar, infelizmente devido as circunstancias não houve muito progresso nesse fonte com nossas aulas praticas e teoricas interrompidas no basico de como fazer e verificar os serviços docker e kubernets
