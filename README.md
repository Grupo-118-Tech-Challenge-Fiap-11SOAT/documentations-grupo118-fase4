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