# documentations-grupo118-fase4

## Diagrama C4 - Contexto do Sistema

```mermaid
C4Context
    title Diagrama de Contexto do Sistema - Tech Challenge Fase 4

    Person(clienteFinal, "Cliente Final", "Usuário que realiza pedidos e pagamentos no sistema")
    Person(usuarioAdmin, "Usuário Administrativo", "Gerencia produtos, pedidos e configurações do sistema")
    
    System(sistema, "Sistema Tech Challenge", "Sistema de gestão de pedidos e pagamentos")
    
    System_Ext(mercadoPago, "Mercado Pago", "Sistema de pagamentos externo para processar transações")

    Rel(clienteFinal, sistema, "Realiza pedidos e consulta status", "HTTPS")
    Rel(usuarioAdmin, sistema, "Gerencia produtos, pedidos e relatórios", "HTTPS")
    Rel(sistema, mercadoPago, "Processa pagamentos", "API REST/HTTPS")
    Rel(mercadoPago, sistema, "Envia notificações de pagamento", "Webhook")

    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
```