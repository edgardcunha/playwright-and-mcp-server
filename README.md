# Playwright e MCP Server: Eleve os testes de aplicações web ao próximo nível!

Automação E2E moderna com C#, arquitetura baseada em contexto e máxima produtividade no VSCode. Repositório da palestra "Playwright e MCP Server: Eleve os testes de aplicações web ao próximo nível!" na Multivix Cachoeiro.

## Etapa 1 - Configure o Ambiente de Desenvolvimento

1. Crie um Repositório no GitHub ou faça um "fork" deste repositório.

2. Se optar por criar seu próprio, na raiz do seu repositório, crie uma pasta chamada `.devcontainer`: no terminal execute `mkdir .devcontainer`.

3. Em seguida, crie um arquivo dentro dessa pasta chamado `devcontainer.json`: no terminal execute `touch .devcontainer/devcontainer.json`.

4. Cole o seguinte conteúdo no arquivo. Este arquivo instrui o GitHub Codespaces sobre como construir seu ambiente:

```json
{
    "name": "Desenvolvimento .NET com Playwright",
    "image": "mcr.microsoft.com/devcontainers/dotnet:8.0",

    // 'Features' adicionam ferramentas e runtimes ao seu ambiente.
    "features": {
        // Instala automaticamente os navegadores e dependências do Playwright.
        "ghcr.io/microsoft/playwright:1": {}
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

## Etapa 2 - Criando uma API Simples com `dotnet`

> Posteriormente, é preciso habilitar a extensão do Copilot.