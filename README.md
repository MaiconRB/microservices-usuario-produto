# microservices-usuario-produto

Solução de exemplo com dois microsserviços .NET independentes — **UsuarioService** e **ProdutoService** — que se comunicam entre si via HTTP. A arquitetura de cada serviço segue camadas de **Clean Architecture** (Core / Application / Infrastructure / API) e os princípios de **SOLID**.

![.NET 8](https://img.shields.io/badge/.NET-8-512BD4?logo=dotnet&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?logo=csharp&logoColor=white)
![EF Core](https://img.shields.io/badge/EF%20Core-SQL%20Server-CC2927?logo=microsoftsqlserver&logoColor=white)
![xUnit](https://img.shields.io/badge/Tests-xUnit%20%2B%20Moq-25A162)

## Serviços

| Serviço | Responsabilidade | Porta (dev) |
|---|---|---|
| **UsuarioService** | CRUD e gestão de usuários | `5009` |
| **ProdutoService** | CRUD de produtos; consulta o dono do produto chamando o `UsuarioService` via `HttpClient` | `5010` |

Cada serviço é organizado em camadas independentes:

```
<Service>/
├── API/Controllers/        # Endpoints HTTP
├── Application/Services/   # Regras de negócio
├── Core/
│   ├── Entities/            # Entidades de domínio
│   └── Interfaces/          # Contratos (Core não depende de nada)
├── Infrastructure/
│   ├── Data/                # DbContext (EF Core)
│   └── Repositories/        # Implementação de acesso a dados
└── Migrations/
```

## Rodando localmente

Pré-requisitos: [.NET 8 SDK](https://dotnet.microsoft.com/download) e SQL Server acessível.

```bash
dotnet restore

# em dois terminais separados
dotnet run --project UsuarioService   # http://localhost:5009
dotnet run --project ProdutoService   # http://localhost:5010
```

Com o `UsuarioService` no ar, o `ProdutoService` consegue resolver o dono de um produto em `GET /api/produto/usuario/{id}`.

## Testes

```bash
dotnet test
```

`UsuarioService.Tests` cobre o `UsuarioService` com testes unitários (xUnit + Moq) e um teste de integração.

## Tecnologias utilizadas

- .NET 8 / C#
- Entity Framework Core + SQL Server
- Comunicação entre serviços via `HttpClient`
- xUnit e Moq para testes
