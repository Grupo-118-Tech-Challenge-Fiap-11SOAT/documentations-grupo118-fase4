# documentations-grupo118-fase4

## Diagrama C4 - Contexto do Sistema

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