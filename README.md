# LaunchScout

LaunchScout é um radar pessoal para descobrir workshops, virtual summits, free lives e produtos de produtores independentes. O catálogo preserva `source`, `discoveredAt` e `registration` em cada lançamento para manter a proveniência do sinal.

## Executar localmente

```bash
npm install
npm run dev
```

O build de produção é gerado com `npm run build` em `dist/`.

## Hospedagem Hostinger ou GitHub Pages

O projeto é uma aplicação estática Vite. No Hostinger, execute `npm ci && npm run build` no ambiente Node ou publique o conteúdo de `dist/` no diretório público configurado. O endpoint, token e banco de lançamentos continuam no WordPress que hospeda o plugin LaunchRadar MCP Bridge; a aplicação não cria tokens fictícios nem possui um banco local simulado.

## MCP

Abra **Configurações MCP** para criar, editar, pausar ou remover vários agentes. Cada agente possui nome, função, endpoint e token próprios. Ao executar uma pesquisa, todos os agentes habilitados são consultados em paralelo via JSON-RPC/MCP, os resultados são normalizados e duplicados são removidos por título, data e produtor. Tokens ficam apenas no `localStorage` deste navegador; em computador compartilhado, revogue o token no WordPress depois do uso.

Para uso real no Hostinger ou GitHub Pages, o endpoint MCP precisa aceitar requisições HTTPS do domínio publicado e permitir o header `Authorization` e a origem via CORS. O LaunchScout chama `initialize` para testar a conexão e `tools/call` com `search_launches` para pesquisar.

## Pesquisa abrangente

O formulário pesquisa por nome do evento/lançamento e texto livre, além de filtros dinâmicos de plataforma/fonte, tipo de evento, produtor, período e interesse. As opções de plataforma, produtor e tipo são construídas a partir dos dados reais recebidos dos agentes. Os filtros são enviados ao plugin como `event_name`, `source`, `product_type`, `producer_name`, `date_window_days` e `query`.

Se todos os agentes falharem, o sistema exibe o erro de conexão e não injeta resultados de demonstração. Verifique endpoint, token, HTTPS e CORS no painel **LaunchRadar → MCP Settings**.
