# Seminário T05 — Kubernetes



Disciplina DevOps Tools  

Professor Diogo Rodrigues



## Integrantes



- Alessandro Mathews Cardoso Dornelas dos Santos

- Eduardo Rafael Silva Santos

- Gabryella Tainá Melo Silva

- Luís Gabriel da Silva Araújo

- Victhor Emanuel Soares Brito



## Objetivo



Aplicar um Deployment e um Service em um cluster Minikube e acessar

uma aplicação web Nginx pelo navegador.



## Arquitetura



O cluster local possui um nó que executa o Control Plane e os Pods

da aplicação.



O Deployment `site` gerencia três réplicas do Nginx. O Service `site`,

do tipo NodePort, seleciona os Pods pela label `app: site`.



Os recursos ficam no namespace `t05-demo`.



## Estrutura



- `k8s/aplicacao.yaml` — configuração Kubernetes.

- `slides/` — apresentação editável e PDF.

- `docs/` — roteiro de apresentação e demonstração.



## Requisitos



- Windows 11, usado como ambiente de referência.

- Virtualização habilitada no firmware.

- WSL 2 configurado.

- Docker Desktop funcionando com backend WSL 2.

- Minikube instalado.

- Internet para os downloads iniciais.



Os comandos deste guia usam PowerShell e o kubectl fornecido pelo Minikube.



## Versões utilizadas

| Ferramenta | Versão |
| --- | --- |
| Windows | Windows 11 Pro 25H2 — build 26200.8037, modificado com FSOS |
| Docker Desktop | 4.100.1 |
| Docker Engine | 29.9.3 |
| WSL | 3.0.5.0 |
| Kernel WSL 2 | 6.18.58.1-microsoft-standard-WSL2 |
| Minikube | 1.39.0 |
| Kubernetes | 1.37.0 |
| Runtime do nó Minikube | containerd 2.3.4 |
| Nginx | 1.30.8 |
| Imagem utilizada | docker.io/library/nginx:stable |
| Digest confirmado | sha256:aabaaeb9c84fc529520a0be49fd000ebae452de4b46fb9906039b69c21508b07 |



Consultar com



```powershell

docker version

minikube version

minikube -p seminario kubectl -- version

```



## Preparação



Abrir o Docker Desktop e aguardar a inicialização.



Verificar o mecanismo Docker.



```powershell

docker info

```



Iniciar o cluster.



```powershell

minikube start -p seminario --driver=docker

```



Conferir o nó.



```powershell

minikube -p seminario kubectl -- get nodes

```



O resultado esperado é o nó `seminario` com status `Ready`.



## Implantação



Na pasta raiz do repositório, executar



```powershell

minikube -p seminario kubectl -- apply -f .\\k8s\\aplicacao.yaml

```



Aguardar a implantação.



```powershell

minikube -p seminario kubectl -- -n t05-demo rollout status deployment/site --timeout=180s

```



## Verificação



Consultar os recursos.



```powershell

minikube -p seminario kubectl -- -n t05-demo get deployment,replicaset,pods,service

```



Resultado esperado



- Deployment `site` com três réplicas prontas.

- Três Pods com `READY 1/1` e status `Running`.

- Service `site` do tipo NodePort.



Conferir as labels e os destinos do Service.



```powershell

minikube -p seminario kubectl -- -n t05-demo get pods --show-labels



minikube -p seminario kubectl -- -n t05-demo get endpointslices -l kubernetes.io/service-name=site

```



## Acesso à aplicação



Em outro terminal, executar



```powershell

minikube service site -p seminario -n t05-demo --url

```



Abrir a URL exibida no navegador.



Resultado esperado — página “Welcome to nginx!”.



Manter esse terminal aberto durante o acesso.

Encerrar com Ctrl+C ao terminar.



## Limpeza



O comando abaixo remove os recursos declarados no arquivo,

incluindo o namespace `t05-demo` e os recursos contidos nele.



```powershell

minikube -p seminario kubectl -- delete -f .\\k8s\\aplicacao.yaml

```



Para parar o cluster mantendo sua configuração



```powershell

minikube stop -p seminario

```



Depois, encerrar o Docker Desktop usando “Quit Docker Desktop”.



## Limitações



- Cluster com um único nó.

- Réplicas compartilham os recursos da mesma máquina.

- Aplicação sem banco de dados ou persistência.

- Acesso local fornecido pelo Minikube.

- Exemplo sem HTTPS e observabilidade completa.

- A tag `nginx:stable` é mutável. Registrar a referência testada e

&#x20; fixar uma versão ou digest antes da entrega final.



## Fontes e créditos



- Kubernetes — https://kubernetes.io/docs/

- Deployments — https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

- Services — https://kubernetes.io/docs/concepts/services-networking/service/

- Minikube — https://minikube.sigs.k8s.io/docs/start/

- Docker Desktop — https://docs.docker.com/desktop/



Registrar os créditos das imagens, modelos e demais materiais utilizados.

