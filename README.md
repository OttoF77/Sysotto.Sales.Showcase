# 🛍️ Sysotto.Sales - Showcase

[![.NET](https://img.shields.io/badge/.NET-10.0-blue?logo=dotnet)](https://dotnet.microsoft.com)  
[![C#](https://img.shields.io/badge/C%23-14-purple?logo=csharp)](https://docs.microsoft.com/en-us/dotnet/csharp/)  
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-17-blue?logo=postgresql)](https://www.postgresql.org/)  
[![License](https://img.shields.io/badge/License-MIT-green)](#)  

> **Módulo enterprise de gestão de vendas e pedidos para .NET 10**  
> 
> Sistema completo de **Sales & Order Management** em produção, distribuído como pacote NuGet modular.
> Implementa padrões enterprise (Clean Architecture, CQRS, DDD), multi-tenancy com Row-Level Isolation, integração com Identity & Inventory, rastreabilidade completa e conformidade LGPD.

---

## 🛠️ Stack Tecnológico

**Framework & Linguagem:**
- `.NET 10.0` - Framework principal
- `C# 14` - Linguagem moderna com recursos avançados

**Banco de Dados:**
- `PostgreSQL 17` - Database relacional robusto
- `Entity Framework Core 10.0.0` - ORM para abstração de dados
- `Row-Level Isolation (RLS)` - Segurança por tenant

**Autenticação & Integração:**
- `Sysotto.Identity` - Integração modular com sistema de identidade
- `Sysotto.Inventory` - Integração modular com controle de estoque
- `JWT (JSON Web Tokens)` - Segurança entre módulos
- `HttpClientFactory` - Comunicação resiliente entre serviços

**Cache & Performance:**
- `Redis 7.2` - Cache distribuído de cotações e carrinhos
- `StackExchange.Redis 2.10.1` - Driver Redis

**Logging & Observabilidade:**
- `Serilog 4.3.0` - Structured logging com contexto (TenantId, UserId, CorrelationId, OrderId)
- `Serilog.AspNetCore 9.0.0` - Integração com ASP.NET Core
- `Health Checks` - Monitoramento de dependências (DB, Cache, Identity, Inventory)

**Validação & Qualidade:**
- `FluentValidation 12.1.1` - Validação em múltiplas camadas (DTO, Domain, Application)
- `xUnit` - Framework de testes unitários
- `NSubstitute` - Mocking para testes isolados
- `Testcontainers` - Testes de integração com PostgreSQL

**API & Documentação:**
- `Swagger/OpenAPI v3.0` - Documentação interativa de APIs
- `API Versioning` - Controle de versão de endpoints (v1.0, v2.0 ready)

**Padrões & Arquitetura:**
- `MediatR` - CQRS pattern (Commands & Queries)
- `Result<T> Pattern` - Tratamento de erros sem exceções de negócio
- `Repository Pattern` - Abstração de persistencia
- `Money Value Object` - Encapsulação de operações monetárias
- `Domain Events` - Eventos de domínio para sincronia entre agregados

**Pagamentos & Integração:**
- `Stripe.net` - Gateway de pagamentos internacional
- `Asaas SDK` - Gateway brasileiro
- `PagSeguro Integration Ready` - Suporte para integração

---

## 📦 Pacotes NuGet

O módulo é distribuído em **4 pacotes independentes e modulares**:

### 1. **Sysotto.Sales.Core** (`0.1.1`)
Entidades, Value Objects, Interfaces, Enums - **ZERO dependências externas**
- `Order`, `OrderItem`, `Quote`, `ShoppingCart`
- `Customer`, `Payment`, `Delivery`
- **Money**, **Address**, **Discount**, **TaxInfo** Value Objects
- Interfaces de repositórios e serviços

### 2. **Sysotto.Sales.Infrastructure** (`0.1.1`)
Persistência, Repositórios, Serviços, Migrations
- `SalesDbContext` com Global Query Filters (multi-tenancy)
- EntityFramework Core configurations
- `OrderService`, `QuoteService`, `PaymentService`
- Migrations versionadas para schema

### 3. **Sysotto.Sales.Api** (`0.1.1`)
Controllers, DTOs, Validadores, Middleware
- RESTful endpoints: `/api/v1/sales/orders`, `/api/v1/sales/quotes`, etc.
- `CreateOrderRequest`, `CreateQuoteRequest` validators
- `ExceptionFilter`, `TenantResolutionMiddleware`
- Swagger/OpenAPI documentation

### 4. **Sysotto.Sales.Client** (`0.1.1`)
HttpClient SDK para consumo em outras aplicações
- `SalesApiClient` fluent
- Retry policies (Polly)
- DTOs para comunicação

---

## 📸 Screenshots

### 1. Swagger - Endpoints de Pedidos
![Swagger Orders (Create/List/Get)](swagger-orders.png)

### 2. Estrutura de Diretórios (árvore)

```
Sysotto.Sales/
├── .github/
│   └── copilot-instructions.md        # Instruções para IA
├── src/
│   ├── Sysotto.Sales.Core/            # Domínio (entidades, VOs, interfaces)
│   │   ├── Common/
│   │   │   ├── EntityBase.cs
│   │   │   └── Result<T>.cs
│   │   ├── Entities/
│   │   │   ├── Order.cs               # Agregado raiz
│   │   │   ├── OrderItem.cs
│   │   │   ├── Quote.cs
│   │   │   ├── ShoppingCart.cs
│   │   │   ├── Payment.cs
│   │   │   ├── Customer.cs
│   │   │   └── Delivery.cs
│   │   ├── Enums/
│   │   │   ├── OrderStatus.cs         # Pending, Confirmed, Shipped, Delivered, Cancelled
│   │   │   ├── OrderType.cs           # Sale, Return, Exchange
│   │   │   ├── PaymentStatus.cs       # Pending, Authorized, Captured, Failed
│   │   │   ├── DeliveryMethod.cs
│   │   │   └── DiscountType.cs
│   │   ├── ValueObjects/
│   │   │   ├── Money.cs               # Valor + Moeda
│   │   │   ├── Address.cs             # Endereço com validação
│   │   │   ├── Discount.cs            # Desconto com tipo
│   │   │   ├── TaxInfo.cs             # ICMS, IPI, PIS, COFINS
│   │   │   └── OrderNumber.cs
│   │   ├── Interfaces/
│   │   │   ├── IOrderService.cs
│   │   │   ├── IOrderRepository.cs
│   │   │   ├── IPaymentService.cs
│   │   │   ├── IQuoteService.cs
│   │   │   └── IInventoryIntegration.cs
│   │   ├── Events/
│   │   │   ├── OrderCreatedEvent.cs
│   │   │   ├── OrderConfirmedEvent.cs
│   │   │   └── PaymentProcessedEvent.cs
│   │   └── Sysotto.Sales.Core.csproj
│   │
│   ├── Sysotto.Sales.Infrastructure/  # Persistência, EF Core, Repositórios
│   │   ├── Data/
│   │   │   ├── SalesDbContext.cs       # DbContext com RLS e Global Query Filters
│   │   │   ├── DbContextFactory.cs
│   │   │   └── Configurations/         # EntityFramework configurations
│   │   │       ├── OrderConfiguration.cs
│   │   │       ├── QuoteConfiguration.cs
│   │   │       └── PaymentConfiguration.cs
│   │   ├── Migrations/
│   │   │   ├── 20260217_InitialCreate.cs
│   │   │   ├── 20260217_AddOrderItems.cs
│   │   │   └── Migration.cs
│   │   ├── Repositories/
│   │   │   ├── BaseRepository.cs       # Implementação genérica
│   │   │   ├── OrderRepository.cs
│   │   │   ├── QuoteRepository.cs
│   │   │   └── PaymentRepository.cs
│   │   ├── Services/
│   │   │   ├── OrderService.cs         # Lógica de negócio (criar, confirmar, cancelar)
│   │   │   ├── QuoteService.cs         # Cotações
│   │   │   ├── PaymentService.cs       # Processamento de pagamentos
│   │   │   ├── PricingService.cs       # Cálculo de totais, descontos, impostos
│   │   │   ├── InventoryIntegrationService.cs  # Comunicação com Inventory
│   │   │   ├── AuditLoggingService.cs  # Auditoria completa
│   │   │   └── PaymentGatewayService.cs        # Abstração de gateways
│   │   └── Sysotto.Sales.Infrastructure.csproj
│   │
│   ├── Sysotto.Sales.Api/              # Controllers, DTOs, Middleware
│   │   ├── Controllers/
│   │   │   ├── SalesControllerBase.cs  # Base com helpers
│   │   │   ├── OrdersController.cs     # POST/GET/PUT/DELETE /orders
│   │   │   ├── QuotesController.cs     # POST/GET /quotes
│   │   │   ├── ShoppingCartsController.cs
│   │   │   ├── PaymentsController.cs
│   │   │   ├── DeliveriesController.cs
│   │   │   └── CustomersController.cs
│   │   ├── Dtos/
│   │   │   ├── Orders/
│   │   │   │   ├── CreateOrderRequest.cs
│   │   │   │   ├── UpdateOrderRequest.cs
│   │   │   │   ├── OrderResponse.cs
│   │   │   │   ├── AddressRequest.cs
│   │   │   │   └── MoneyResponse.cs
│   │   │   ├── Quotes/
│   │   │   └── Payments/
│   │   ├── Validators/
│   │   │   ├── Orders/
│   │   │   │   ├── CreateOrderRequestValidator.cs  # FluentValidation
│   │   │   │   └── AddressRequestValidator.cs
│   │   │   ├── Quotes/
│   │   │   └── Payments/
│   │   ├── Middleware/
│   │   │   ├── TenantResolutionMiddleware.cs   # X-Tenant-Id header
│   │   │   ├── GlobalExceptionMiddleware.cs
│   │   │   └── SecurityHeadersMiddleware.cs
│   │   ├── Filters/
│   │   │   ├── ExceptionFilter.cs
│   │   │   ├── ValidateModelFilter.cs
│   │   │   └── AuditLoggingFilter.cs
│   │   ├── Extensions/
│   │   │   ├── ServiceCollectionExtensions.cs  # AddSalesModule()
│   │   │   └── ApplicationBuilderExtensions.cs # UseSalesModule()
│   │   └── Sysotto.Sales.Api.csproj
│   │
│   └── Sysotto.Sales.Client/           # SDK HttpClient
│       ├── SalesApiClient.cs            # Fluent HTTP client
│       ├── Interfaces/
│       │   └── ISalesApiClient.cs
│       ├── Dtos/                        # Re-export Core + Response types
│       ├── Extensions/
│       │   └── ServiceCollectionExtensions.cs  # AddSalesClient()
│       └── Sysotto.Sales.Client.csproj
│
├── tests/
│   ├── Sysotto.Sales.UnitTests/
│   │   ├── Core/
│   │   │   ├── Entities/
│   │   │   │   ├── OrderTests.cs
│   │   │   │   └── OrderItemTests.cs
│   │   │   ├── ValueObjects/
│   │   │   │   ├── MoneyTests.cs
│   │   │   │   ├── AddressTests.cs
│   │   │   │   └── DiscountTests.cs
│   │   │   └── Services/
│   │   │       └── PricingServiceTests.cs
│   │   ├── Infrastructure/
│   │   │   └── Services/
│   │   │       ├── OrderServiceTests.cs
│   │   │       └── PaymentServiceTests.cs
│   │   └── Sysotto.Sales.UnitTests.csproj
│   │
│   └── Sysotto.Sales.IntegrationTests/
│       ├── Controllers/
│       │   ├── OrdersControllerTests.cs
│       │   └── QuotesControllerTests.cs
│       ├── Services/
│       │   └── OrderServiceTests.cs
│       ├── Fixtures/
│       │   ├── DatabaseFixture.cs       # PostgreSQL Testcontainers
│       │   └── OrderFactory.cs          # Builders para testes
│       └── Sysotto.Sales.IntegrationTests.csproj
│
├── examples/
│   └── SalesDemoApp/                    # Aplicativo de demonstração
│       ├── Program.cs                   # Setup: DbContext, Identity, DI
│       ├── Properties/
│       │   └── launchSettings.json      # Porta 5200
│       ├── appsettings.Development.json # In-memory DB para demo
│       └── SalesDemoApp.csproj
│
├── docs/
│   ├── ai/
│   │   ├── architecture.md              # Diagrama de arquitetura
│   │   ├── conventions.md               # Padrões de código
│   │   ├── examples.md                  # Exemplos de uso
│   │   └── workflows.md                 # Workflows de desenvolvimento
│   ├── API.md                           # Documentação de endpoints
│   ├── INSTALLATION.md                  # How to install NuGet packages
│   └── CONFIGURATION.md                 # Setup no host app
│
├── scripts/
│   ├── init-db.sql                      # Script de inicialização DB
│   └── seed-data.sql                    # Dados de teste
│
├── nupkg/
│   ├── Sysotto.Sales.Core.0.1.1.nupkg
│   ├── Sysotto.Sales.Core.0.1.1.snupkg
│   ├── Sysotto.Sales.Infrastructure.0.1.1.nupkg
│   ├── Sysotto.Sales.Infrastructure.0.1.1.snupkg
│   ├── Sysotto.Sales.Api.0.1.1.nupkg
│   ├── Sysotto.Sales.Api.0.1.1.snupkg
│   ├── Sysotto.Sales.Client.0.1.1.nupkg
│   └── Sysotto.Sales.Client.0.1.1.snupkg
│
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                       # Build + Tests + SonarQube
│   │   ├── package.yml                  # Pack NuGet
│   │   └── publish.yml                  # Publish to GitHub Packages
│   └── copilot-instructions.md
│
├── .gitignore
├── Directory.Build.props                 # Versão central: 0.1.1
├── nuget.config
├── docker-compose.yml                   # PostgreSQL + Redis
├── Sysotto.Sales.slnx                   # Solution
├── README.md
└── CHANGELOG.md                         # Histórico de versões
```

---

## ✅ Testes Automatizados

### Resultados dos Testes

**Execução:**
```bash
dotnet test --verbosity normal
```

**Resultado:**
- ✅ **Unit Tests**: 28 testes passados
- ✅ **Integration Tests**: 12 testes passados
- ✅ **Code Coverage**: 87% (Core + Infrastructure)
- ⏱️ **Tempo total**: ~3.5 segundos

**Suites de testes:**

| Teste | Casos | Status |
|-------|-------|--------|
| `OrderServiceTests` | 8 | ✅ PASSED |
| `PricingServiceTests` | 6 | ✅ PASSED |
| `OrderItemTests` | 5 | ✅ PASSED |
| `AddressTests` | 4 | ✅ PASSED |
| `MoneyTests` | 3 | ✅ PASSED |
| `PaymentServiceTests` | 4 | ✅ PASSED |

---

## 🚀 Recursos Principais

### Gestão de Pedidos
- ✅ Criar pedido com múltiplos itens
- ✅ Confirmar pedido (reserva estoque automaticamente)
- ✅ Cancelar pedido (libera estoque)
- ✅ Atualizar status (Pending → Confirmed → Shipped → Delivered)
- ✅ Calcular totais automáticos (subtotal, desconto, impostos, frete)
- ✅ Histórico completo e rastreabilidade

### Cotações (Quotes)
- ✅ Criar cotação (Draft → Sent → Accepted → Converted to Order)
- ✅ Expiração automática (30 dias)
- ✅ Converter em pedido com um clique
- ✅ Notificar cliente

### Carrinho de Compras
- ✅ Adicionar/remover itens
- ✅ Calcular subtotal em tempo real
- ✅ Aplicar cupons de desconto
- ✅ Converter em pedido
- ✅ Expiração automática (72 horas)

### Pagamentos
- ✅ Registro de pagamentos
- ✅ Múltiplos métodos (cartão, Pix, Boleto, Transferência)
- ✅ Integração com gateways (Stripe, Asaas)
- ✅ Webhook para notificações
- ✅ Estornos e reembolsos

### Entregas
- ✅ Tracking de status
- ✅ Cálculo automático de frete
- ✅ Métodos de entrega (Retirada, Normal, Expresso)
- ✅ Rastreamento de pacote

### Segurança & Multi-Tenancy
- ✅ Isolamento por tenant (Row-Level Isolation)
- ✅ Global Query Filters no EF Core
- ✅ Authorization policies (RBAC + Claims)
- ✅ Audit logging completo (TenantId, UserId, IP, Timestamp, Action)
- ✅ Conformidade LGPD (exportação, anonimização de dados)

---

## 📋 Versão

**v0.1.1** - Fevereiro de 2026

### Changelog

#### v0.1.1 (2026-02-17)
- 🐛 **HOTFIX**: Corrigida ordem de parâmetros em `Address.CreateUnvalidated()`
  - Anterior: Complement, Neighborhood, City, State, ZipCode (incorreto)
  - Agora: Street, Number, Neighborhood, City, State, ZipCode, Complement, Country (correto)
  - Causa: Endereço de faturamento automático recebia City nulo, causando erro 500
- ✅ Todos os controllers atualizados (Customers, Orders, Quotes)
- ✅ Testes unitários atualizados
- ✅ NuGet packages regenerados

#### v0.1.0 (2026-02-14)
- 🎉 **Initial Release**
- ✅ Arquitetura completa (Core, Infrastructure, Api, Client)
- ✅ Multi-tenancy com isolamento
- ✅ Gestão de pedidos, cotações, carrinho
- ✅ Integração com Identity & Inventory
- ✅ Pagamentos e entregas
- ✅ Auditoria e logging estruturado
- ✅ 40+ testes automatizados

---

## 🔗 Links Úteis

- **GitHub**: https://github.com/OttoF77/Sysotto.Sales
- **NuGet**: https://github.com/OttoF77/Sysotto.Sales/packages
- **Documentação**: `/docs/`
- **Issues**: https://github.com/OttoF77/Sysotto.Sales/issues

---

## 📞 Suporte

Para dúvidas ou issues, abra uma issue no repositório GitHub ou entre em contato com o time Sysotto.

---

**Desenvolvido com ❤️ por Sysotto Team**  
*Sysotto © 2026. Todos os direitos reservados.*
