| Integrante | Participação |
|---|---|
| Eduardo | Slides 1 e 2, abertura da prática no slide 8 |
| Alessandro | Slides 3 e 4, arquitetura na prática no slide 8 |
| Victhor | Slide 5 e aplicação do YAML no slide 8 |
| Luís | Slide 6 e acesso à aplicação no slide 9 |
| Gabryella | Slide 7, interpretação no slide 9 e conclusão no slide 10 |

### Slide 1 — Kubernetes na prática
**Eduardo**

> “Bom dia! Somos Eduardo, Alessandro, Victhor, Luís e Gabryella e vamos apresentar o tema Kubernetes.
>
> Nosso objetivo é implantar uma aplicação web em um cluster local, usando um Deployment e um Service, e acessar essa aplicação pelo navegador.
>
> Primeiro vamos apresentar o problema, os conceitos e a arquitetura. Depois faremos uma demonstração ao vivo, mostrando a configuração, os comandos e os resultados.
>
> A aplicação escolhida é o Nginx, um servidor web que permite verificar o funcionamento por meio de uma página no navegador.”

### Slide 2 — Problema e caso de uso
**Eduardo**

> “Imaginem que uma equipe precisa disponibilizar uma aplicação web. Precisamos definir qual imagem será executada, quantas cópias queremos e como acessar essas cópias.
>
> Também precisamos registrar a configuração para que outro integrante consiga reproduzir a implantação em seu computador.
>
> Vamos resolver isso declarando os recursos em um arquivo YAML. O Deployment descreve a execução da aplicação, enquanto o Service oferece um ponto de acesso aos Pods.
>
> Nosso resultado esperado será um Deployment com três réplicas prontas, um Service associado a esses Pods e a página do Nginx acessível.
>
> O README da equipe registra os requisitos, as versões, os comandos e as verificações para reproduzir o exemplo.”

**Passagem**

> “Para entender como essa configuração vira uma aplicação em execução, vamos conhecer os principais conceitos do Kubernetes.”

### Slide 3 — Fundamentos do Kubernetes
**Alessandro**

> “Kubernetes é uma plataforma de orquestração de aplicações em containers. Ele coordena a execução dessas aplicações em um conjunto de máquinas chamado cluster.
>
> Cada máquina do cluster é um nó. Os containers são executados dentro de Pods, que são as menores unidades implantáveis do Kubernetes.
>
> Um Pod pode conter um ou mais containers. No nosso exemplo, cada Pod terá um container do Nginx.
>
> Outro conceito central é o estado desejado. Quando configuramos três réplicas, declaramos quantas queremos manter. Os controladores observam o estado atual e trabalham para aproximá-lo dessa configuração.
>
> Isso permite administrar a aplicação por meio de uma descrição declarativa.”

### Slide 4 — Arquitetura e fluxo
**Alessandro**

> “A arquitetura é dividida entre o Control Plane, responsável pelo gerenciamento, e os componentes que executam as aplicações nos nós.
>
> O API Server recebe as solicitações enviadas pelo kubectl. O etcd armazena os dados do cluster. O scheduler escolhe um nó adequado para os Pods que ainda precisam ser atribuídos.
>
> O controller-manager executa controladores que acompanham os recursos, incluindo Deployment e ReplicaSet.
>
> No nó, o kubelet acompanha os Pods atribuídos e utiliza o runtime para executar os containers.
>
> O fluxo começa com o envio da configuração à API. Os controladores providenciam os recursos, o scheduler atribui os Pods ao nó e o kubelet coordena sua execução.
>
> Em nossa demonstração, o Minikube reúne o gerenciamento e a execução da aplicação em um único nó.”

**Passagem**

> “Agora vamos detalhar o Deployment, que descreve como nossa aplicação deve executar.”

### Slide 5 — Deployment
**Victhor**

> “O Deployment é o recurso que usamos para declarar a execução da aplicação.
>
> No nosso YAML, definimos a imagem do Nginx e solicitamos três réplicas. O campo template descreve a configuração dos Pods, incluindo o container, sua porta e suas labels.
>
> O Deployment gerencia um ReplicaSet, que mantém a quantidade desejada de Pods.
>
> As labels são identificações associadas aos recursos. Usamos app igual a site para identificar os Pods desta aplicação. O selector do Deployment corresponde a essa identificação.
>
> Durante a demonstração, vamos conferir essa relação no terminal, visualizando o Deployment, seu ReplicaSet e os três Pods.
>
> Como o ambiente tem um único nó, todas as réplicas compartilham os recursos dessa máquina.”

**Passagem**

> “Com a aplicação em execução, precisamos oferecer uma forma de acessá-la. Essa será a função do Service.”

### Slide 6 — Service
**Luís**

> “Cada Pod possui seu endereço de rede, mas os Pods podem ser substituídos. O Service oferece um ponto de acesso estável enquanto esse recurso existir.
>
> Ele identifica os Pods por meio de um selector. Neste exemplo, selecionamos os Pods com a label app igual a site.
>
> O campo port define a porta oferecida pelo Service. O targetPort indica a porta de destino nos Pods. Utilizamos o nome http, declarado na configuração do container.
>
> Escolhemos o tipo NodePort. Para acessar a aplicação no Windows com o driver Docker, usaremos o comando minikube service, que disponibiliza uma URL local.
>
> Na prática, vamos comparar o selector com as labels e abrir essa URL no navegador.”

### Slide 7 — Ambiente e decisões
**Gabryella**

> “Escolhemos o Minikube para executar o Kubernetes localmente. O Docker fornece o ambiente usado pelo driver do Minikube.
>
> O Nginx oferece uma aplicação pequena, cujo resultado podemos verificar pelo navegador.
>
> A configuração fica em YAML para permitir revisão e reprodução. Os recursos estão agrupados no namespace t05-demo.
>
> Registramos no README as versões utilizadas, os requisitos, a preparação e os passos de execução.
>
> Nosso ambiente já estará iniciado antes da apresentação. Durante a demonstração, vamos aplicar a configuração e interpretar o resultado de cada etapa.”

**Passagem**

> “Agora vamos acompanhar a implantação ao vivo.”

### Slide 8 — Prática: implantação
**Eduardo, Alessandro e Victhor**

Deixem o **Terminal A** aberto na pasta `seminario-kubernetes`. O Docker e o cluster devem estar iniciados.

**Eduardo — Mostrar o ambiente**

```powershell
minikube version
minikube -p seminario kubectl -- get nodes
```

> “Este é o cluster que usaremos. Seu nome é seminario e ele possui um nó com status Ready.
>
> As versões do ambiente estão registradas no README. Agora vamos observar os componentes que gerenciam esse cluster.”

**Alessandro — Mostrar a arquitetura**

```powershell
minikube -p seminario kubectl -- -n kube-system get pods
```

Aponte para API Server, etcd, scheduler e controller-manager.

> “Aqui localizamos os componentes apresentados no diagrama.
>
> Nossos próximos comandos serão enviados à API. Os controladores e os componentes do nó vão trabalhar para executar a configuração solicitada.
>
> Neste cluster, o mesmo nó também executará os Pods da aplicação.”

**Victhor — Mostrar e aplicar o YAML**

```powershell
Get-Content .\k8s\aplicacao.yaml
```

> “O arquivo declara o namespace, o Deployment e o Service.
>
> Vamos destacar as três réplicas, a imagem do Nginx, a label app igual a site e a configuração da porta. Essas informações descrevem o estado que queremos obter.”

```powershell
minikube -p seminario kubectl -- apply -f .\k8s\aplicacao.yaml
```

> “O apply envia a configuração ao cluster. A saída informa quais recursos foram criados, configurados ou já estavam de acordo com o arquivo.”

```powershell
minikube -p seminario kubectl -- -n t05-demo rollout status deployment/site --timeout=180s

minikube -p seminario kubectl -- -n t05-demo get deployment,replicaset,pods
```

> “Agora verificamos o resultado. O Deployment deve apresentar três réplicas prontas, e cada Pod deve mostrar 1/1 na coluna Ready.
>
> Também podemos observar o ReplicaSet criado e gerenciado pelo Deployment.”

**Passagem**

> “Com os Pods prontos, vamos verificar o Service e acessar a aplicação.”

### Slide 9 — Prática: acesso e verificação
**Luís e Gabryella**

**Luís — Conferir o Service**

```powershell
minikube -p seminario kubectl -- -n t05-demo describe service site

minikube -p seminario kubectl -- -n t05-demo get pods --show-labels
```

> “O Service utiliza o selector app igual a site. Aqui conferimos que essa identificação corresponde às labels dos Pods.
>
> Também vemos a porta oferecida pelo Service e a porta de destino da aplicação.”

No **Terminal B**, execute e mantenha aberto.

```powershell
minikube service site -p seminario -n t05-demo --url
```

Abra a URL exibida no navegador.

> “O Minikube disponibilizou este endereço local. Ao acessá-lo, vemos a página de boas-vindas do Nginx.
>
> Esse terminal permanece aberto para manter o acesso fornecido pelo Minikube.”

**Gabryella — Interpretar os resultados**

No Terminal A, execute

```powershell
minikube -p seminario kubectl -- -n t05-demo get endpointslices -l kubernetes.io/service-name=site
```

> “Os EndpointSlices registram os endereços dos destinos associados ao Service.
>
> Temos três evidências do resultado. O Deployment possui as réplicas prontas, o Service possui destinos associados e a página está acessível pelo navegador.
>
> Essas verificações permitem avaliar tanto o estado dos recursos quanto o acesso à aplicação.”

### Slide 10 — Limitações, conclusão e fontes
**Gabryella**

> “Concluímos o objetivo do T05, aplicando um Deployment e um Service no Minikube e acessando a aplicação.
>
> Nosso ambiente possui um único nó. As três réplicas compartilham essa máquina, então o exemplo não demonstra tolerância à perda do nó.
>
> Também usamos uma aplicação simples, sem banco de dados, HTTPS ou observabilidade completa.
>
> A entrega contém os slides editáveis e em PDF, o YAML e o README com preparação, versões, execução, verificação, limpeza e referências.
>
> As fontes principais são as documentações oficiais do Kubernetes e do Minikube. Obrigado! Estamos disponíveis para as perguntas.”
