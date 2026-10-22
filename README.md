# 📊 NEXUS OS Omega V3

Um dashboard de **monitoramento de sistemas operacionais** que transforma informações do servidor em indicadores, gráficos e painéis interativos. O NEXUS OS Omega V3 foi desenvolvido com **Node.js e Express** e apresenta, em português, dados sobre recursos do sistema, processos e comportamento da própria aplicação web.

## ✨ O que o painel oferece

| Área | Recursos disponíveis |
|---|---|
| **CPU e memória** | Uso do processador, informações por núcleo, RAM e consumo de memória do Node.js. |
| **Sistema e rede** | Plataforma, arquitetura, hostname, tempo de atividade e interfaces de rede. |
| **Processos** | Visualização, pesquisa e ordenação de processos em modo somente leitura. |
| **Runtime e HTTP** | Indicadores do event loop, heap, versões do ambiente e métricas das requisições. |
| **Análise visual** | Gráficos, histórico de amostras, alertas e indicadores de saúde do ambiente. |
| **Relatórios** | Exportação de dados em JSON, sessão em CSV e comparação entre relatórios importados. |

A interface inclui **cinco temas visuais**, controles de apresentação e uma paleta de comandos acessível por **Ctrl+K**. As informações podem chegar por **Server-Sent Events (SSE)** ou por consultas periódicas.

## 🛠️ Tecnologias utilizadas

| Tecnologia | Aplicação |
|---|---|
| **Node.js** | Coleta de métricas e execução do servidor. |
| **Express** | Rotas HTTP e entrega da interface. |
| **Helmet** | Configuração de cabeçalhos HTTP de segurança. |
| **HTML, CSS e JavaScript** | Construção da interface interativa. |

## 📂 Estrutura do projeto

| Arquivo | Responsabilidade |
|---|---|
| [`server.js`](server.js) | Servidor, coleta de informações, rotas e interface do dashboard. |
| [`package.json`](package.json) | Dependências, scripts e requisitos de execução. |
| [`package-lock.json`](package-lock.json) | Registro das versões das dependências npm. |
| [`.gitignore`](.gitignore) | Regras de exclusão de arquivos do Git. |

## 🌐 Rotas do servidor

| Endpoint | Finalidade |
|---|---|
| `/` | Abre o dashboard. |
| `/api/dashboard` | Retorna informações de monitoramento em JSON. |
| `/api/public-snapshot` | Fornece um resumo das métricas. |
| `/api/stream` | Transmite atualizações via SSE. |
| `/api/health` | Retorna informações sobre o estado do serviço. |

## ▶️ Como executar

O projeto declara suporte a **Node.js 18 ou superior**. Com o npm disponível:

```bash
npm ci
npm start
```

O servidor utiliza a porta **3000** por padrão, ou o valor da variável `PORT`. Para desenvolvimento, o manifesto também oferece `npm run dev`.

> 💡 Quando o dashboard está hospedado na nuvem, as métricas exibidas são do **servidor ou contêiner que executa a aplicação**, não do dispositivo de quem acessa a página.

## 📚 Referências

[Node.js — módulo `os`](https://nodejs.org/api/os.html) · [MDN — Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) · [Render — Node.js e Express](https://render.com/docs/deploy-node-express-app)
