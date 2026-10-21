# NEXUS OS Omega V3 — Dashboard de monitoramento

Aplicação em português para acompanhar informações do sistema operacional, do processo Node.js e das requisições recebidas pelo servidor. O projeto usa Express e Helmet e concentra o servidor, as APIs e a interface no arquivo [server.js](server.js).

As métricas correspondem ao ambiente em que o Node.js está em execução. Quando o servidor está na nuvem, o painel apresenta os recursos disponíveis nesse ambiente.

## Conteúdo do repositório

| Arquivo | Conteúdo |
| --- | --- |
| [server.js](server.js) | Coleta de métricas, rotas HTTP, HTML, CSS e JavaScript da interface. |
| [package.json](package.json) | Versão do projeto, dependências, scripts e versão mínima do Node.js. |
| [package-lock.json](package-lock.json) | Versões das dependências registradas pelo npm. |
| [.gitignore](.gitignore) | Regras para ignorar dependências, arquivos de ambiente e logs. |

## Instalação

O [package.json](package.json) declara **Node.js 18 ou superior**, Express e Helmet. Com Node.js e npm disponíveis, execute na raiz do repositório:

```bash
npm ci
npm start
```

O servidor utiliza a porta **3000** por padrão e escuta em `0.0.0.0`. Para escolher outra porta em um terminal Bash:

```bash
PORT=3001 npm start
```

Para reiniciar o servidor automaticamente após alterações em `server.js`:

```bash
npm run dev
```

## Informações apresentadas

| Área | Informações disponíveis |
| --- | --- |
| CPU | Uso médio, uso por núcleo, média e pico recentes, modelo, frequência e carga média. |
| Memória | RAM total, livre e usada; RSS, heap e outras informações de memória do processo Node.js. |
| Sistema operacional | Nome da máquina, plataforma, versão do sistema, arquitetura e tempo de atividade. |
| Runtime | Versões do Node.js e do V8, atraso e utilização do event loop, heap e eventos de coleta de lixo. |
| Rede | Interfaces e endereços identificados pelo módulo `os`. |
| HTTP | Quantidade de requisições, duração média das respostas, classes de status e taxa de erros. |
| Projeto | Quantidade e tamanho dos arquivos da aplicação, com exclusão de `node_modules` e `.git`. |
| Armazenamento | Espaço do volume da aplicação, quando a informação está disponível. |
| Processos | PID, nome, CPU e memória dos processos que o sistema permite consultar. |
| Sessão | Médias, picos, percentis e desvios em relação às amostras recentes. |

A pontuação de saúde e os alertas são calculados por regras definidas no código. Eles ajudam a interpretar o painel, mas não constituem um teste de desempenho controlado.

## Recursos da interface

- Cinco temas: Cyber Cyan, Quantum Violet, Matrix Emerald, Crimson Reactor e Solar Gold.
- Neon suave, intenso ou desligado.
- Modos de observação e apresentação, tela cheia e controle de efeitos visuais.
- Pausa e retomada da atualização.
- Paleta de comandos com `Ctrl+K` ou `Cmd+K`; navegação pelas teclas de seta, execução com Enter e fechamento com Esc.
- Filtro de processos por nome ou PID e ordenação por CPU, memória, nome ou PID.
- Exportação e cópia de relatórios em JSON.
- Importação de dois relatórios para comparação.
- Gravação de amostras no navegador e exportação da sessão em CSV.

Preferências como tema, neon e perfil de atualização são guardadas em `localStorage`. Os relatórios exportados usam o formato `nexus-os-report-v1`.

## Atualização das informações

O perfil padrão utiliza SSE, com envio de telemetria a cada **1,5 segundo**. A interface também oferece perfis de consulta a cada 1, 3 ou 5 segundos. Se a conexão SSE falhar, o código passa a consultar a API periodicamente.

A CPU é amostrada a cada segundo. A lista de processos é atualizada a cada cinco segundos e mostra até 14 itens. Os históricos permanecem em memória durante a execução.

## Rotas HTTP

| Método | Caminho | Resposta |
| --- | --- | --- |
| GET | `/` | Página HTML do dashboard. |
| GET | `/api/dashboard` | Telemetria completa em JSON, com os campos `success`, `timestamp` e `data`. |
| GET | `/api/public-snapshot` | Resumo da telemetria em JSON. |
| OPTIONS | `/api/public-snapshot` | Resposta de preflight com status 204. |
| GET | `/api/stream` | Fluxo SSE com eventos `telemetry` e, em caso de falha na coleta, `fault`. |
| GET | `/api/health` | Estado de disponibilidade do servidor, com `success: true` e `status: "online"`. |

Com o servidor em execução, estas consultas locais permitem conferir as respostas:

```bash
curl --fail --silent http://127.0.0.1:3000/api/health
curl --fail --silent http://127.0.0.1:3000/api/dashboard
curl --fail --silent http://127.0.0.1:3000/api/public-snapshot
```

Ajuste a porta se tiver definido outro valor em `PORT`.

## Comparação com uma instância remota

O painel NEXUS LINK possui um campo para informar a URL de outra instância e consultá-la por meio de `/api/public-snapshot`.

Há uma divergência no código atual: a função `publicSnapshot` produz o esquema `nexus-os-live-v3`, enquanto `fetchRemote` aceita `nexus-os-live-v2`. Assim, duas instâncias desta versão não concluem a conexão remota entre si. A comparação por importação de relatórios JSON utiliza um formato separado.

## Execução no Render

Para o Web Service descrito no material do projeto, use os comandos:

| Configuração | Comando |
| --- | --- |
| Instalação das dependências | `npm ci` |
| Inicialização do serviço | `npm start` |

O servidor lê a variável `PORT`, que pode ser fornecida pela plataforma.

## Apresentação no OBS

O material do projeto propõe usar a página como uma fonte do tipo **Navegador** no OBS Studio. Inicie a aplicação antes de configurar a fonte e use **Interagir** para acessar os controles da página.

## Observações

- Chart.js, Lucide e as fontes da interface são carregados por serviços externos. Sem Chart.js, o código mantém as informações textuais e não cria os gráficos.
- A lista de processos usa PowerShell no Windows e `ps` no Linux. Se a consulta não estiver disponível, o painel mostra apenas o processo Node.js.
- O gerenciador de tarefas permite apenas a consulta de informações: não oferece controles para encerrar processos.
- Ao reiniciar o servidor, os históricos mantidos em memória são descartados.
- Os scripts do [package.json](package.json) são `start` e `dev`; o projeto não define um script de testes.
