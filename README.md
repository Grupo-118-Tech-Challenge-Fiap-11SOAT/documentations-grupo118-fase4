# documentations-grupo118-fase4
Criamos um repositório separado para armazenar diversos detalhes acerca da implementação do projeto na Fase 4

# Membros do Grupo
- Sabrina Cardoso de Oliveira
    - **Matrícula**: RM363507
    - **Usuário Discord**: sah.mdo
- Tiago Cristiano Koch
    - **Matrícula**: RM361415
    - **Usuário Discord**: tiagokoch0076
- Tiago Victor de Oliveira
    - **Matrícula**: RM364588
    - **Usuário Discord**: oliveirad.tiago
- Túlio Henrique de Paula Rezende
    - **Matrícula**: RM360982
    - **Usuário Discord**: tuliomamute
- Vinícius Rossmann Nunes
    - **Matrícula**: RM362963
    - **Usuário Discord**: _viniciusnunes

# Vídeo de Apresentação do Projeto

Para assistir ao vídeo de apresentação do projeto, clique na imagem abaixo:

[![Watch the video](11SOAT%20-%20Fase%204%20-%20Grupo%20118.PNG)]()

# Separação da aplicação em microserviços
Dividimos a aplicação original em alguns microsserviços de maneira a paralelizarmos o desenvolvimento e facilitar a manutenção futura do sistema. 

Como parte da definição da atividade informava que a lanchonete era algo em crescimento ainda, preferimos utilizar um modelo de comunicação simples, baseado em requisições HTTP REST entre os microsserviços, evitando nesse primeiro momento uma complexidade adicional. Porém, em algums pontos da aplicação existe a possibilidade de ajustes futuros para utilização de modelo de eventos, como por exemplo:
- na comunicação entre o serviço de pedidos e pagamentos, onde a utilização de filas poderia trazer mais robustez ao sistema.
- na comunicaçação entre o serviço de pagamentos e pedidos, onde o serviço de pagamentos poderia publicar eventos de mudança de status de pagamento, e o serviço de pedidos poderia se inscrever nesses eventos para atualizar o status dos pedidos.

A seguir estão os microsserviços criados:

## 1. Módulo de Produtos

- Repositório: [tech-challenge-grupo-118-products-fase-4](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/tech-challenge-grupo-118-products-fase-4)

- Abordagem da API: Clean Architecture

![Code Coverage - Products.png](Products/Code%20Coverage%20-%20Products.png)

Criamos um microsserviço dedicado à gestão do catálogo de produtos.
Utilizamos **MongoDB** para armazenar os detalhes dos produtos, aproveitando sua flexibilidade para permitir que cada tipo de produto tenha atributos específicos, enriquecendo uma eventual visualização em um catalógo. Abaixo temos alguns exemplos de produtos inseridos em nossa base para demonstrar esse modelo flexível:

### Snack

```json
{
  "_id": {
    "$oid": "695ed9ec2a6b7bb7f043c025"
  },
  "_t": [
    "Product",
    "Snack"
  ],
  "name": "X-Salada",
  "price": {
    "$numberDecimal": "12"
  },
  "isActive": true,
  "images": [],
  "createdAt": {
    "DateTime": {
      "$date": "2026-01-07T22:10:52.095Z"
    },
    "Ticks": {
      "$numberLong": "639034206520956483"
    },
    "Offset": 0
  },
  "updatedAt": {
    "DateTime": {
      "$date": "2026-01-07T22:10:52.095Z"
    },
    "Ticks": {
      "$numberLong": "639034206520956718"
    },
    "Offset": 0
  },
  "ingredients": [
    "Pão",
    "Queijo",
    "Hamburgues"
  ]
}
```

### Drink

```json
{
  "_id": {
    "$oid": "695ed9ec2a6b7bb7f043c02c"
  },
  "_t": [
    "Product",
    "Drink"
  ],
  "name": "Pepsi",
  "price": {
    "$numberDecimal": "12"
  },
  "isActive": true,
  "images": [],
  "createdAt": {
    "DateTime": {
      "$date": "2026-01-07T22:10:52.099Z"
    },
    "Ticks": {
      "$numberLong": "639034206520991314"
    },
    "Offset": 0
  },
  "updatedAt": {
    "DateTime": {
      "$date": "2026-01-07T22:10:52.099Z"
    },
    "Ticks": {
      "$numberLong": "639034206520991316"
    },
    "Offset": 0
  },
  "size": "M",
  "flavor": null
}
```

## 2. Módulo de Pedidos

- Repositório: [tech-challenge-grupo-118-orders-fase-4](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/tech-challenge-grupo-118-orders-fase-4)

- Abordagem da API: Hexagonal

![Code Coverage - Orders.png](Orders/Code%20Coverage%20-%20Orders.png)

Criamos um microsserviço dedicado à gestão de pedidos.
Utilizamos **SQL Server** para armazenar os dados dos pedidos, aproveitando a estrutura relacional para garantir a integridade dos dados e facilitar consultas, como por exemplo a visualização dos pedidos para monitoramento em telas para a ordem de entrega.

Iríamos em um primeiro momento, utilizar o banco PostgreSQL, porém por questões de infraestrutura (a impossibilidade de criação de um cluster na região East US, onde o nosso cluster AKS está hospedado) optamos por utilizar o SQL Server, que já estava disponível em nossa infraestrutura na Azure e já era o modelo utilizado no monolito.

## 3. Módulo de Pagamentos

- Repositório: [tech-challenge-grupo-118-payments-fase-4](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/tech-challenge-grupo-118-payments-fase-4)

- Abordagem da API: Clean Architecture com Minimal APIs

![Code Coverage - Payments.png](Payments/Code%20Coverage%20-%20Payments.png)

Criamos um microsserviço dedicado ao processamento de pagamentos.
Utilizamos **SQL Server** para armazenar os dados dos pagamentos para manter a compatibilidade com a implementação anterior e ganharmos tempo durante o processo de desenvolvimento.

O módulo de pagamentos se comunica com a API do MercadoPago para processar as transações financeiras, informando a URL do webhook para receber notificações de status de pagamento.

## 4. Módulo de Usuários

- Repositórios: [tech-challenge-grupo-118-users-fase-4](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/tech-challenge-grupo-118-users-fase-4) e [TechChallengeFastFoodFunction](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/TechChallengeFastFoodFunction)

- Abordagem da API: Hexagonal

![Code Coverage - Users.png](Users/Code%20Coverage%20-%20Users.png)

![Code Coverage - Lambda.png](Users/Code%20Coverage%20-%20Lambda.png)

Criamos um microsserviço dedicado à gestão de usuários e aproveitamos a Lambda function criada na fase anterior para efetivar o login e geração do JWT para uso.
Também utilizamos **SQL Server** para armazenar os dados dos usuários e customers, mantendo a compatibilidade com a implementação anterior.

# Desenho da infraestrutura geral

No desenho abaixo ilustramos como é a estrutura dos recursos na Azure para suportar a aplicação.

![1 - Kubernetes Architecture - Tech Challenge - Abordagem Produtiva - Fase 4.png](1%20-%20Kubernetes%20Architecture%20-%20Tech%20Challenge%20-%20Abordagem%20Produtiva%20-%20Fase%204.png)

- Cliente/usuário administrativo efetuam uma requisição
- Requisição chega no APIM
- APIM roteia, via NGINX Ingress Controller interno, para o serviço correto no cluster AKS
- Serviço no AKS se comunica com o banco de dados correspondente (MongoDB Atlas ou SQL Server na Azure)

# Processo de Deploy e infraestrutura como código

Para facilitar o deploy e garantir um processo padronizado entre os membros do grupo, criamos templates de pipeline de CI e CD utilizando Github Actions

- Repositório Template de Pipeline: [terraform-template-pipeline-grupo118-fase-3](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/terraform-template-pipeline-grupo118-fase-3)
- Repositório Template de Helm Charts: [helm-chart-grupo118-fase-4](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/helm-chart-grupo118-fase-4)
- Repositório do Terraform de Infra: [k8s-terraform-infra-grupo-118-fase-3](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/k8s-terraform-infra-grupo-118-fase-3)
- Repositório do Terraform de Banco de Dados: [database-terraform-infra-grupo-118-fase-3](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/database-terraform-infra-grupo-118-fase-3)

## Continous Integration
No processo de Continous Integration (CI), cada microsserviço possui um pipeline configurado no **Github Actions** que realiza as seguintes etapas:

### Durante o pull request
- Compilação do código: validação do build do projeto em um ambiente limpo
- Execução dos testes automatizados: executa todos os projetos de teste contidos na solução
- Análise de qualidade de código com SonarCloud: o template de pipeline cria automaticamente o projeto no SonarCloud caso ele não exista, utilizando as variáveis de ambiente configuradas no repositório
  - Caso o quality gate não atinja o percentual de 80%, é inserido no PR um comentário automático informando a falha na qualidade do código e o mesmo fica bloqueado para merge.
- Build da imagem Docker: Validação se o build em um contexto de container ocorre corretamente.

- Build e análise de código durante o pull request
![CI - Build e Analise - PR.png](CI/CI%20-%20Build%20e%20Analise%20-%20PR.png)

- Build Docker - PR
![CI - Build Docker - PR.png](CI/CI%20-%20Build%20Docker%20-%20PR.png)

### Na main
Além de todos os passos acima, na main o pipeline realiza também:
- Push da imagem Docker para o Container Registry: a imagem é enviada para o Azure Container Registry (ACR) para ser utilizada posteriormente no deploy.

- Build Docker - Main
  ![CI - Build Docker - Main.png](CI/CI%20-%20Build%20Docker%20-%20Main.png)

## Continous Deployment e Helm Charts
Mantendo a mesma filosofia de padronização, criamos um helm chart que também é salvo no ACR para que todas as aplicações o utilizem. Esse Helm chart é responsável por:
- Criar a estrutura classica para funcionamento em Kubernetes (deployments, services, secrets)
- Criação do Ingress interno para comunicação via APIM
- Criação de Secret de ACR para permitir pull de imagens privadas

E para o deploy temos um pipeline padrão, também via Github Actions, que realiza as seguintes etapas:
- Mapeamento de Github secrets para variáveis de ambiente do pipeline
- Login no ACR
- Pull do Helm Chart
- Proceso de substituição de variáveis no Helm Chart
- Deploy via Helm no cluster AKS

Desse modo, a configuração das variaveis que cada aplicação utiliza ficou de maneira bem flexível

- Arquivo de pipeline de deploy de products como exemplo: [cd.yml](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/tech-challenge-grupo-118-products-fase-4/blob/main/.github/workflows/cd.yml)
- Arquivo de Helm Chart de products utilizado como exemplo: [values-production.yaml](https://github.com/Grupo-118-Tech-Challenge-Fiap-11SOAT/tech-challenge-grupo-118-products-fase-4/blob/main/helm/values-production.yaml)

No momento do deploy, precisamos apenas informar qual a versão do Helm chart e qual a versão da imagem gera

![Disparo do workflow.png](CD/Disparo%20do%20workflow.png)

Feito isso, o processo acontece de forma automática

![Execução do deploy.png](CD/Execu%C3%A7%C3%A3o%20do%20deploy.png)

## Terraform

Na parte de infraestrutura, reaproveitamos o repositório criado na fase 3 para manter a infraestrutura como código utilizando Terraform, visto que em termos de arquitetura de recursos, não tivemos mudanças.
Já na parte de banco de dados, também reaproveitamos o terraform de banco de dados, porém acrescentando os bancos de cada serviço e um módulo novo de MongoDB Atlas para o serviço de produtos.

Ambos os deploys foram realizados via Github Actions.

- Deploy da infraestrutura regular
![Deploy Infra.png](Terraform/Deploy%20Infra.png)

- Deploy da infraestrutura de banco de dados
![Deploy Infra de Banco de Dados.png](Terraform/Deploy%20Infra%20de%20Banco%20de%20Dados.png)

## Secrets
Todos os pipelines reaproveitam alguns secrets configurados no Github Actions para garantir a segurança das credenciais utilizadas durante o processo de CI/CD.
- Credenciais Azure
- Credenciais ACR
- Credenciais SonarCloud

Além disso, cada repositório de microsserviço possui seus próprios secrets para utilização no momento do deploy, como: 
- Strings de conexão com banco de dados
- Chaves de API do MercadoPago
- Configuração de JWT

# Configurações
Alguns passos, manuais até então, são necessários para o funcionamento correto do sistema:
1. Conexão no Kubernetes via kubectl port-foward para buscar o Swagger 
   - Como as APIs não são expostas publicamente, é preciso importar manualmente o Swagger para o APIM (efetuando o download do mesmo). Para isso, precisamos acessar as APIs via kubectl port-forward

Exemplo para o módulo de produtos:

```bash
kubectl port-forward svc/tech-challenge-grupo-118-products-fase-4 8080:80 -n tech-challenge
```

2. Importação do Swagger no APIM
   - Após baixar o Swagger via port-forward, é necessário importar o mesmo no APIM para que as APIs fiquem disponíveis para consumo
   - No portal do APIM, selecionar "APIs" e clicar em "+ Add API"
   - Selecionar "OpenAPI" e fazer o upload do arquivo Swagger baixado
   - Após o upload, configurar o "Web service URL" para o endpoint correto (exemplo: http://10.10.0.10/products-api)
     - é feito dessa maneira pois o Ingress configurado está pronto para substituir e enviar corretamente para as APIs, o path substituido.

![Configuração de APIM.png](Publica%C3%A7%C3%A3o/Configura%C3%A7%C3%A3o%20de%20APIM.png)

3. Configuração no serviço de Pedidos da URL do APIM
  - Como enviamos a URL, no momento da criação do pagamento, para o MercadoPago, precisamos garantir que essa URL seja a do APIM e não a do serviço diretamente.
  - Essa URL é configurada via secrets do github no repositório de payments (secret: GS_MERCADOPAGO_NOTIFICATION_URL)

# Diagramas

Abaixo elaboramos alguns diagramas C4 para ilustrar a arquitetura do sistema.

## Diagrama C4 - Contexto do Sistema - Sistema de Pedidos

```mermaid
C4Context
    title Diagrama de Contexto do Sistema - Tech Challenge

    Person(clienteFinal, "Cliente Final", "Usuário que realiza pedidos e pagamentos no sistema")
    Person(usuarioAdmin, "Usuário Administrativo", "Gerencia produtos, pedidos e configurações do sistema")
    
    Boundary(b1, "Camada de Aplicação") {
        System(sistemaTC, "Sistema Tech Challenge", "Sistema de gestão de pedidos,produtos e pagamentos")
    }
    
    Boundary(b2, "Serviços Externos") {
        System_Ext(mercadoPago, "Mercado Pago", "Plataforma de pagamentos externo para processar transações")
    }

    Rel(clienteFinal, sistemaTC, "Realiza pedidos")
    Rel(usuarioAdmin, sistemaTC, "Gerencia sistema")
    BiRel(mercadoPago, sistemaTC, "Processa pagamentos / Notifica Status")

    UpdateRelStyle(clienteFinal, sistemaTC, $offsetY="-40", $offsetX="10")    
    UpdateRelStyle(mercadoPago, sistemaTC, $offsetY="-20", $offsetX="10")
    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")

```

## Diagrama C4 - Visão de Container - Sistema de Pedidos

```mermaid
C4Container
    title Diagrama de Container - Sistema de Pedidos Tech Challenge

    Person(customer, "Usuário Final", "Cliente que realiza pedidos no sistema")
    Person(admin, "Usuário Administrativo", "Gerencia produtos, usuários e pedidos")

    Container_Boundary(sistema, "Sistema de Pedidos") {
        Container(ui, "Terminal de Atendimento", "Web Application", "Interface de autoatendimento para clientes realizarem pedidos e pagamentos")
        
        Container(adminUI, "UI Administrativa", "Web Application", "Interface administrativa para gestão de produtos, usuários e pedidos")
        
        Container(backend, "Backend", "Microsserviços", "Fornece funcionalidades do sistema via API REST, incluindo gestão de pedidos, produtos, usuários e pagamentos")
        
        ContainerDb(database, "Database", "SQL e MongoDB", "Armazena informações de pedidos, produtos, usuários e logs do sistema")
    }

    Enterprise_Boundary(external, "Sistemas Externos") {
        System_Ext(mercadopago, "MercadoPago API", "Sistema de pagamento externo para processamento de transações")
    }

    Rel(customer, ui, "Realiza pedidos e pagamentos usando", "HTTPS")
    Rel(admin, adminUI, "Gerencia produtos, usuários e pedidos usando", "HTTPS")
    
    Rel(ui, backend, "Faz chamadas API para", "JSON/HTTPS")
    Rel(adminUI, backend, "Faz chamadas API para", "JSON/HTTPS")
    
    Rel(backend, database, "Lê e escreve dados", "SQL/MongoDB Protocol")
    Rel(backend, mercadopago, "Processa pagamentos via", "JSON/HTTPS")
    Rel(mercadopago, backend, "Notifica status de pagamento via", "Webhook/HTTPS")

    UpdateRelStyle(customer, ui, $offsetY="-50", $offsetX="-20")
    UpdateRelStyle(admin, adminUI, $offsetY="-50", $offsetX="-50")
    UpdateRelStyle(backend, database, $offsetX="-50", $offsetY="-30")
    UpdateRelStyle(mercadopago, backend, $offsetX="-240", $offsetY="-20")
    UpdateRelStyle(backend, mercadopago, $offsetX="80", $offsetY="-20")
    
    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
```


## Diagrama C4 - Visão de Componentes - Backend do Sistema de Pedidos

```mermaid
C4Component
    title Diagrama de Componentes - Backend do Sistema de Pedidos Tech Challenge

    Container(terminal, "Terminal de Atendimento", "Web Application", "Interface de autoatendimento")
    Container(adminUI, "UI Administrativa", "Web Application", "Interface administrativa")

    Container_Boundary(backend, "Backend") {
        
        Boundary(productsModule, "Módulo de Produtos") {
            Component(productsAPI, "Products API", "API REST", "Gerencia catálogo de produtos")
            ComponentDb(productsDB, "Products Database", "MongoDB", "Armazena detalhes dos produtos")
        }

        Boundary(ordersModule, "Módulo de Pedidos") {
            Component(ordersAPI, "Orders API", "API REST", "Gerencia pedidos e orquestra comunicação")
            ComponentDb(ordersDB, "Orders Database", "SQL Server", "Armazena dados de pedidos")
        }
        
        Boundary(paymentsModule, "Módulo de Pagamentos") {
            Component(paymentsAPI, "Payments API", "API REST", "Processa pagamentos e integra com MercadoPago")
            ComponentDb(paymentsDB, "Payments Database", "SQL Server", "Armazena pagamentos criados")
        }
        
        Boundary(usersModule, "Módulo de Usuários") {
            Component(usersAPI, "Users e Customers API", "API REST", "Gerencia cadastro de usuários")
            Component(authFunction, "Azure Function Users", "Serverless Function", "Efetiva login e criação de usuários")
            ComponentDb(usersDB, "Users Database", "SQL Server", "Armazena usuários e customers")
        }
    }

    System_Ext(mercadopago, "MercadoPago API", "Gateway de pagamento externo")

    Rel(terminal, ordersAPI, "Cria e consulta pedidos", "JSON/HTTPS")
    Rel(adminUI, productsAPI, "Gerencia produtos", "JSON/HTTPS")
    Rel(adminUI, usersAPI, "Gerencia usuários", "JSON/HTTPS")
    Rel(adminUI, ordersAPI, "Gerencia pedidos", "JSON/HTTPS")
    
    Rel(ordersAPI, productsAPI, "Busca detalhes dos produtos", "JSON/HTTPS")
    BiRel(ordersAPI, paymentsAPI, "Solicita pagamento / Notifica status", "JSON/HTTPS")
    Rel(ordersAPI, ordersDB, "Lê e grava pedidos", "SQL")
    
    Rel(productsAPI, productsDB, "Lê e grava produtos", "MongoDB Protocol")
    
    Rel(paymentsAPI, paymentsDB, "Lê e grava pagamentos", "SQL")
    BiRel(paymentsAPI, mercadopago, "Processa pagamento / Webhook status", "JSON/HTTPS")
    
    Rel(usersAPI, usersDB, "Lê e grava usuários", "SQL")
    
    Rel(authFunction, usersDB, "Valida credenciais e cria usuários", "SQL")

    UpdateRelStyle(terminal, ordersAPI, $textColor="yellow" ,$offsetY="-60", $offsetX="-220")
    UpdateRelStyle(adminUI, productsAPI,$textColor="purple", $offsetY="-115", $offsetX="-40")
    UpdateRelStyle(adminUI, usersAPI,$textColor="purple", $offsetY="-80", $offsetX="-280")
    UpdateRelStyle(adminUI, ordersAPI,$textColor="purple", $offsetY="-60", $offsetX="-30")
    
    UpdateRelStyle(ordersAPI, productsAPI,$textColor="green", $offsetY="-40", $offsetX="-80")
    UpdateRelStyle(ordersAPI, paymentsAPI, $lineColor="red",$textColor="green", $offsetY="20", $offsetX="-85")
    UpdateRelStyle(ordersAPI, ordersDB, $lineColor="orange", $textColor="orange", $offsetY="-10", $offsetX="-10")
    
    UpdateRelStyle(productsAPI, productsDB, $lineColor="orange", $textColor="orange",  $offsetY="-10", $offsetX="10")
    
    UpdateRelStyle(paymentsAPI, paymentsDB,$lineColor="orange", $textColor="orange", $offsetY="20", $offsetX="50")
    UpdateRelStyle(paymentsAPI, mercadopago, $lineColor="red",$textColor="green", $offsetY="-120", $offsetX="-60")
    
    UpdateRelStyle(usersAPI, usersDB, $lineColor="orange", $textColor="orange", $offsetY="-100", $offsetX="0")
    
    UpdateRelStyle(authFunction, usersDB,$lineColor="orange", $textColor="orange", $offsetY="-10", $offsetX="10")
    
    UpdateLayoutConfig($c4ShapeInRow="4", $c4BoundaryInRow="4")
```