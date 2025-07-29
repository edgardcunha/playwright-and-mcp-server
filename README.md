# Playwright e MCP Server: Eleve os testes de aplicações web ao próximo nível!

Automação E2E moderna com C#, arquitetura baseada em contexto e máxima produtividade no VSCode. Repositório da palestra "Playwright e MCP Server: Eleve os testes de aplicações web ao próximo nível!" na Multivix Cachoeiro.

## Etapa 1 - Configure o Ambiente de Desenvolvimento

1. Crie um Repositório no GitHub ou faça um "fork" deste repositório.

2. Automatize a configuração do ambiente de desenvolvimento utilizando o GitHub Codespaces. Você pode usar o arquivo `devcontainer.json` já existente neste repositório ou criar o seu próprio.
- Se optar por criar seu próprio, na raiz do seu repositório, crie uma pasta chamada `.devcontainer`: no terminal execute `mkdir .devcontainer`.
- Em seguida, crie um arquivo dentro dessa pasta chamado `devcontainer.json`: no terminal execute `touch .devcontainer/devcontainer.json`.
- Cole o seguinte conteúdo no arquivo. Este arquivo instrui o GitHub Codespaces sobre como construir seu ambiente:

```json
{
    "name": "Desenvolvimento .NET com Playwright",
    "image": "mcr.microsoft.com/devcontainers/dotnet:8.0",

    // 'Features' adicionam ferramentas e runtimes ao seu ambiente.
    "features": {
        // Instala automaticamente os navegadores e dependências do Playwright.
    },

    // Encaminha as portas da nossa API para serem acessíveis no navegador.
    "forwardPorts": [5001, 5000],

    // Define configurações específicas do VSCode.
    "customizations": {
        "vscode": {
            // Instala as extensões essenciais automaticamente.
            "extensions": [
                "ms-dotnettools.csdevkit",
                "ms-playwright.playwright",
                "eamodio.gitlens"
            ]
        }
    },

    // Comando executado após a criação do contêiner.
    "postCreateCommand": "dotnet restore"
}
```
3. Inicie o Codespace:
- Faça o commit desses arquivos (`.devcontainer/devcontainer.json`) no seu repositório.
- Agora, clique no botão `<> Code` > aba `"Codespaces"` e clique em `"Create codespace on main"`.
- O GitHub irá ler seu `devcontainer.json` e construir um ambiente sob medida, já incluindo o SDK do .NET, os navegadores do Playwright e as extensões do VSCode. Toda a configuração manual do ambiente foi eliminada.


## Etapa 2 - Criando uma API Simples com `dotnet`

Dentro do terminal do seu Codespace, vamos criar uma API web mínima que servirá de base para nossos testes.

1. Crie a Solução e o Projeto da API:

```bash
# Cria uma nova solução para organizar nossos projetos
dotnet new sln -n App

# Cria um projeto de API web mínima
dotnet new webapi -minimal -o src/MyApi -n MyApi

# Adiciona o projeto da API à solução
dotnet sln App.sln add src/MyApi/MyApi.csproj
```

2. Adicione um Endpoint Simples:
Abra o arquivo `src/MyApi/Program.cs` e adicione um endpoint de "login" simulado. Este endpoint não fará uma autenticação real, mas retornará uma mensagem de sucesso para nossos testes.

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "API is running!");

// Endpoint de login simulado
app.MapPost("/login", (LoginRequest req) => {
    if (req.Username == "teste@email.com" && req.Password == "senha123")
    {
        return Results.Ok(new { message = "Bem-vindo, Usuário Teste!" });
    }
    return Results.Unauthorized();
});

app.Run();

// DTO para a requisição de login
public record LoginRequest(string Username, string Password);
```

3. Execute a API:

```bash
cd src/MyApi
dotnet run
```

O Codespaces encaminhará a porta automaticamente, conforme definido no `devcontainer.json`.

## Etapa 3: Configurando o Projeto de Testes

Com a API rodando, vamos configurar nosso projeto de teste e o VSCode com as extensões recomendadas na apresentação.

1. Instale as Extensões Recomendadas:

No seu Codespace (que usa VSCode), vá até a aba de Extensões e instale:

- C# Dev Kit : Oferece uma experiência de IDE aprimorada para C#.
- Playwright Test for VSCode : Essencial para executar e depurar testes Playwright diretamente na IDE.
- GitLens : Ótima para insights sobre o histórico do Git (sugerida na apresentação)

2. Crie o Projeto de Teste:

```bash
# Navegue de volta para a raiz do projeto
cd ../..

# Cria um projeto de teste com NUnit e Playwright
dotnet new nunit -o tests/MyApi.Tests -n MyApi.Tests
cd tests/MyApi.Tests
dotnet add package Microsoft.Playwright.NUnit

# Instala os navegadores do Playwright
pwsh bin/Debug/netX.X/playwright.ps1 install 

# Adiciona o projeto de teste à solução
cd ../..
dotnet sln ModernDevApp.sln add tests/MyApi.Tests/MyApi.Tests.csproj
```