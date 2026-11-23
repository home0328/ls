# LaunchScout — Guia de entrega

## 1. O que é

LaunchScout é um radar pessoal para descobrir lançamentos independentes, incluindo workshops, virtual summits, free lives e produtos digitais. A interface organiza os sinais em cards visuais e permite pesquisar, filtrar, ordenar e salvar oportunidades.

O nome **LaunchScout** substitui o nome usado no briefing original.

## 2. Funcionalidades incluídas

- Dashboard com visão geral do radar, próximos sinais e leitura de tendências.
- Catálogo visual com nome, tipo, data, produtor, imagem, resumo, tags, fonte e link de inscrição.
- Busca textual por título, produtor, tipo, resumo e tags.
- Filtros por tipo, período, produtor e interesse.
- Ordenação por relevância, data mais próxima, descoberta recente e lista pessoal.
- Favoritos persistidos no navegador.
- Descoberta simulada com estado de carregamento e feedback visual.
- Estado vazio para combinações de filtros sem resultado.
- Configuração de múltiplas conexões MCP com criação, edição, pausa, ativação e remoção.
- Consulta ao endpoint MCP ativo via JSON-RPC `tools/call`, usando `search_launches`, autorização Bearer e CORS.
- Persistência de configurações e favoritos em `localStorage`.
- Interface responsiva para desktop, tablet e mobile.

## 3. Estrutura do projeto

```text
launchscout/
├── index.html                 Entrada HTML e metadados
├── package.json               Scripts e dependência do Vite
├── package-lock.json          Lockfile de instalação
├── app.config.ts              Branding do projeto Webdev
├── README.md                  Instruções rápidas
├── TODO.md                    Critérios e estado da implementação
├── docs/
│   └── LAUNCHSCOUT-GUIDE.md   Este guia
├── public/
│   └── manus-routes.json      Manifesto de rotas Webdev
├── src/
│   ├── main.js                Dados, estado, renderização e interações
│   └── styles.css             Direção visual Noir Signal
└── dist/                      Build estático pronto para publicação
```

## 4. Execução local

É necessário ter Node.js instalado.

```bash
npm install
npm run dev
```

O servidor de desenvolvimento escuta na porta 3000. Para gerar o build:

```bash
npm run build
```

A saída fica em `dist/`.

## 5. Publicação no GitHub Pages

O projeto é uma aplicação estática Vite. Uma Action do GitHub pode executar `npm ci` e `npm run build`, e então publicar o diretório `dist/` usando GitHub Pages.

As credenciais não devem ser gravadas no código-fonte. Na versão atual, os tokens inseridos na interface são mantidos apenas no `localStorage` do navegador do usuário. Para maior segurança em produção, recomenda-se um proxy backend ou um servidor MCP intermediário.

## 6. Configuração MCP

1. Abra **Configurações MCP** na barra lateral.
2. Clique em **Adicionar outra** para criar uma fonte.
3. Informe um nome, o endpoint MCP e, se necessário, o token Bearer.
4. Salve a conexão.
5. Use **Buscar novidades** para tentar consultar a fonte ativa.

Para funcionar em uma página publicada no GitHub Pages, o endpoint precisa:

- aceitar HTTPS;
- permitir a origem do domínio publicado via CORS;
- permitir o header `Authorization`;
- aceitar requisições do navegador;
- responder em um formato compatível com o transporte configurado.

Quando a fonte não responde, o LaunchScout mantém a experiência disponível em modo demo com dados locais. Quando a fonte responde sem resultados, a interface informa que não foram encontrados lançamentos e não inventa registros.

## 7. Modelo de dados de lançamento

Cada lançamento usa os principais campos abaixo:

```js
{
  id,
  title,
  producer,
  type,
  date,
  discoveredAt,
  source,
  sourceType,
  registration,
  image,
  summary,
  tags,
  saved,
  status
}
```

`source` identifica a origem do sinal, `discoveredAt` registra quando ele foi encontrado e `registration` mantém o link de inscrição. Essa proveniência deve ser preservada quando um agente de IA normalizar novos dados.

## 8. Direção visual

A interface usa a direção **Noir Signal**: fundo azul quase preto, verde-lima como cor de ação e estado, acentos violeta, laranja e azul para categorizar sinais, além de Space Grotesk e DM Mono para reforçar a sensação de painel de inteligência editorial.

## 9. Validação realizada

- `npm run build` executado com sucesso.
- `GET /manus-routes.json` validado com HTTP local.
- Preview verificado em desktop e mobile.
- Arquivos essenciais e diretório `dist/` verificados antes do empacotamento.
