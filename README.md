# 📊 Relatório técnico — NEXUS OS Omega V3

## 📌 Apresentação do projeto

Este repositório contém o **NEXUS OS Omega V3**, um painel web de observabilidade desenvolvido em Node.js e Express. A aplicação reúne informações sobre o ambiente de execução, o processo Node.js e as requisições recebidas pelo servidor. A versão declarada no projeto é **3.1.0**.

## 📂 Arquivos do repositório

| Arquivo | Responsabilidade |
|---|---|
| [`server.js`](server.js) | Implementa coleta de métricas, rotas HTTP e interface em HTML, CSS e JavaScript. |
| [`package.json`](package.json) | Define dependências, versão mínima do Node.js e comandos de execução. |
| [`package-lock.json`](package-lock.json) | Registra as versões das dependências. |
| [`.gitignore`](.gitignore) | Define arquivos e pastas que o Git deve ignorar. |

A aplicação concentra a maior parte das funcionalidades em `server.js`, sem separar o frontend em arquivos próprios.

## 🧠 Resumo das funcionalidades

| Área | Conteúdo apresentado |
|---|---|
| Processador e memória | Indicadores de CPU, RAM e consumo de memória do processo Node.js. |
| Sistema operacional | Plataforma, arquitetura, nome da máquina, tempo de atividade e interfaces de rede. |
| Ambiente de execução | Versões do Node.js e V8, heap e indicadores do event loop. |
| Requisições HTTP | Volume, duração e categorias de respostas do próprio serviço. |
| Processos | Consulta, pesquisa e ordenação dos processos visíveis, em modo somente leitura. |
| Interface e relatórios | Gráficos, temas visuais, exportações JSON e CSV e comparação por importação de relatórios. |

A interface utiliza atualizações periódicas e eventos **SSE (Server-Sent Events)**. Os principais endpoints implementados são:

| Rota | Finalidade |
|---|---|
| `/` | Apresentar o painel. |
| `/api/dashboard` | Fornecer a telemetria em JSON. |
| `/api/public-snapshot` | Disponibilizar um resumo dos indicadores. |
| `/api/stream` | Transmitir eventos de telemetria. |
| `/api/health` | Informar a disponibilidade do servidor. |

## ⚙️ Execução do projeto

O manifesto exige **Node.js 18 ou superior**. Com npm instalado, a partir da raiz:

```bash
npm ci
npm start
```

O painel usa a porta 3000 por padrão e respeita a variável `PORT`. Para desenvolvimento, há o comando `npm run dev`.

## ⚠️ Observações técnicas

- As métricas correspondem ao **servidor que executa o Node.js**, inclusive quando ele está hospedado na nuvem.
- Os históricos do servidor são temporários e permanecem em memória.
- As rotas de telemetria não exigem autenticação e podem expor informações do ambiente.
- O NEXUS LINK apresenta incompatibilidade entre o esquema `nexus-os-live-v3` produzido pelo servidor e o esquema `nexus-os-live-v2` esperado pelo cliente remoto. A comparação por importação de relatórios é um recurso distinto.
- Gráficos e alguns elementos visuais dependem de bibliotecas e fontes externas.

## 📚 Referências

[Código-fonte](server.js) · [Manifesto npm](package.json) · [Node.js — módulo `os`](https://nodejs.org/api/os.html) · [MDN — SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) · [Render — deploy Node/Express](https://render.com/docs/deploy-node-express-app)
