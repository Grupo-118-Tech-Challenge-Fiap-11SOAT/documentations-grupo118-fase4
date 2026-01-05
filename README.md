# documentations-grupo118-fase4

Repositório dedicado a conter diagramas C4 do sistema desenvolvido para a fase 4.

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
    Rel(sistemaTC, mercadoPago, "Processa pagamentos")
    Rel(mercadoPago, sistemaTC, "Notifica status")

    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")

    UpdateRelStyle(clienteFinal, sistemaTC, $offsetY="-40", $offsetX="10")
    
    UpdateRelStyle(sistemaTC, mercadoPago, $offsetY="-20", $offsetX="20")

    UpdateRelStyle(mercadoPago, sistemaTC, $offsetY="-20", $offsetX="-100")
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
    UpdateRelStyle(mercadopago, backend, $offsetX="-200", $offsetY="-20")
    UpdateRelStyle(backend, mercadopago, $offsetX="50", $offsetY="-20")
    
    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
```


## Diagrama C4 - Visão de Componentes - Backend do Sistema de Pedidos

```mermaid
C4Component
    title Diagrama de Componentes - Backend do Sistema de Pedidos Tech Challenge

    Container(terminal, "Terminal de Atendimento", "Web Application", "Interface de autoatendimento")
    Container(adminUI, "UI Administrativa", "Web Application", "Interface administrativa")

    Container_Boundary(backend, "Backend") {
        
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
        
        Boundary(productsModule, "Módulo de Produtos") {
            Component(productsAPI, "Products API", "API REST", "Gerencia catálogo de produtos")
            ComponentDb(productsDB, "Products Database", "MongoDB", "Armazena detalhes dos produtos")
        }
    }

    System_Ext(mercadopago, "MercadoPago API", "Gateway de pagamento externo")

    Rel(terminal, ordersAPI, "Cria e consulta pedidos", "JSON/HTTPS")
    Rel(adminUI, productsAPI, "Gerencia produtos", "JSON/HTTPS")
    Rel(adminUI, usersAPI, "Gerencia usuários", "JSON/HTTPS")
    Rel(adminUI, ordersAPI, "Consulta pedidos", "JSON/HTTPS")
    
    Rel(ordersAPI, productsAPI, "Busca detalhes dos produtos", "JSON/HTTPS")
    Rel(ordersAPI, paymentsAPI, "Solicita criação de pagamento", "JSON/HTTPS")
    Rel(ordersAPI, ordersDB, "Lê e grava pedidos", "SQL")
    
    Rel(productsAPI, productsDB, "Lê e grava produtos", "MongoDB Protocol")
    
    Rel(paymentsAPI, paymentsDB, "Lê e grava pagamentos", "SQL")
    Rel(paymentsAPI, mercadopago, "Integra para processar pagamento", "JSON/HTTPS")
    Rel(paymentsAPI, ordersAPI, "Notifica atualização de pagamento", "JSON/HTTPS")
    
    Rel(usersAPI, usersDB, "Lê e grava usuários", "SQL")
    
    Rel(authFunction, usersDB, "Valida credenciais e cria usuários", "SQL")
    Rel(authFunction, usersAPI, "Sincroniza dados de usuários", "JSON/HTTPS")
    
    Rel(mercadopago, paymentsAPI, "Envia webhook de status", "JSON/HTTPS")

    UpdateRelStyle(terminal, ordersAPI, $offsetY="-60", $offsetX="-100")
    UpdateRelStyle(adminUI, productsAPI, $offsetY="-60", $offsetX="-50")
    UpdateRelStyle(adminUI, usersAPI, $offsetY="-80", $offsetX="100")
    UpdateRelStyle(adminUI, ordersAPI, $offsetY="-60", $offsetX="50")
    
    UpdateRelStyle(ordersAPI, productsAPI, $offsetY="-50", $offsetX="-80")
    UpdateRelStyle(ordersAPI, paymentsAPI, $offsetY="-60", $offsetX="100")
    UpdateRelStyle(ordersAPI, ordersDB, $offsetY="20", $offsetX="50")
    
    UpdateRelStyle(productsAPI, productsDB, $offsetY="20", $offsetX="50")
    
    UpdateRelStyle(paymentsAPI, paymentsDB, $offsetY="20", $offsetX="50")
    UpdateRelStyle(paymentsAPI, mercadopago, $offsetY="-80", $offsetX="120")
    UpdateRelStyle(paymentsAPI, ordersAPI, $offsetY="60", $offsetX="-120", $textColor="red", $lineColor="red")
    
    UpdateRelStyle(usersAPI, usersDB, $offsetY="40", $offsetX="0")
    
    UpdateRelStyle(authFunction, usersDB, $offsetY="40", $offsetX="0")
    UpdateRelStyle(authFunction, usersAPI, $offsetY="-30", $offsetX="0")
    
    UpdateRelStyle(mercadopago, paymentsAPI, $offsetY="80", $offsetX="-120", $textColor="red", $lineColor="red")
    
    UpdateLayoutConfig($c4ShapeInRow="4", $c4BoundaryInRow="3")
```