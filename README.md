# QA Copilot n8n

Workflow do n8n para apoiar atividades de QA com agentes especializados, roteamento inteligente por intenção e uso de uma base de conhecimento em Google Sheets.

## Visão geral

O QA Copilot recebe uma pergunta pelo chat do n8n, consulta uma base de conhecimento e direciona a solicitação para o agente mais adequado. A resposta é gerada em português e segue formatos padronizados para facilitar documentação, revisão e execução de testes.

## Principais recursos

- Chat público pelo node `When chat message received`.
- Enriquecimento de contexto com dados do Google Sheets.
- Router LLM para classificar a intenção da pergunta.
- Agentes especializados para QA funcional, API, requisitos, segurança, performance, automação e dúvidas gerais.
- Memória de conversa por sessão.
- Modelo OpenAI configurado como `gpt-5-mini`.

## Agentes disponíveis

| Intenção | Agente | Quando usar |
| --- | --- | --- |
| `test-designer` | QA Test Designer | Casos de teste, cenários, Gherkin, BDD, regressão, smoke e massa de teste. |
| `api-expert` | API Expert | APIs REST, endpoints, payloads, headers, autenticação, contratos e ferramentas como Bruno ou Rest Assured. |
| `story-reviewer` | Story Reviewer | Revisão de user stories, critérios de aceite, requisitos e refinamento. |
| `security-qa` | Security QA | LGPD, OWASP, autenticação, autorização, vulnerabilidades, logs e auditoria. |
| `performance-qa` | Performance QA | Testes de carga, stress, spike, volume, resiliência, K6 e JMeter. |
| `general-qa` | General QA | Perguntas gerais sobre qualidade, estratégia de testes e boas práticas. |
| `Automation Architect` | Automation Architect | Arquitetura de automação, padrões, stack, CI/CD e organização de projetos de teste. |

## Estrutura do repositório

```text
.
+-- data/
|   `-- qa-knowledge-base.csv
+-- workflows/
|   `-- qa-copilot.json
`-- README.md
```

## Base de conhecimento

O arquivo [data/qa-knowledge-base.csv](data/qa-knowledge-base.csv) contém uma versão local da base usada pelo workflow. No n8n, o node `Get row(s) in sheet` está configurado para buscar os dados em uma planilha do Google Sheets com a aba `knowledge`.

Colunas esperadas:

- `palavra_chave`
- `categoria`
- `titulo`
- `conteudo`
- `tags`
- `exemplo_pergunta`

## Como pegar o workflow no GitHub

Repositório do projeto:

- [github.com/FelipeMedeiros1/qa-copilot-n8n](https://github.com/FelipeMedeiros1/qa-copilot-n8n)

Arquivo do workflow:

- [workflows/qa-copilot.json](https://github.com/FelipeMedeiros1/qa-copilot-n8n/blob/main/workflows/qa-copilot.json)

Para baixar o arquivo pelo GitHub:

1. Abra o arquivo [workflows/qa-copilot.json](https://github.com/FelipeMedeiros1/qa-copilot-n8n/blob/main/workflows/qa-copilot.json).
2. Clique em `Raw`.
3. Salve a página como `qa-copilot.json`.
4. Use esse arquivo na importação do n8n.

URL raw para importação direta:

```text
https://raw.githubusercontent.com/FelipeMedeiros1/qa-copilot-n8n/main/workflows/qa-copilot.json
```

## Como importar no n8n

Acesse o n8n:

- [https://app.n8n.cloud](https://app.n8n.cloud)
- [https://n8n.io](https://n8n.io)

### Opção 1: Importar por arquivo

1. Abra o n8n.
2. Crie um novo workflow.
3. Clique no menu de três pontos do workflow.
4. Selecione `Import from file`.
5. Escolha o arquivo `qa-copilot.json` baixado do GitHub.
6. Configure as credenciais necessárias.
7. Revise o node `Get row(s) in sheet` e selecione a planilha/aba correta.
8. Ative o workflow.
9. Abra o chat público gerado pelo trigger e envie uma pergunta de teste.

### Opção 2: Importar por URL

1. Abra o n8n.
2. Crie um novo workflow.
3. Clique no menu de três pontos do workflow.
4. Selecione `Import from URL...`.
5. Cole a URL raw do workflow:

```text
https://raw.githubusercontent.com/FelipeMedeiros1/qa-copilot-n8n/main/workflows/qa-copilot.json
```

6. Confirme a importação.
7. Configure as credenciais necessárias.
8. Revise o node `Get row(s) in sheet` e selecione a planilha/aba correta.
9. Ative o workflow.
10. Abra o chat público gerado pelo trigger e envie uma pergunta de teste.

## Credenciais necessárias

- OpenAI API, usada pelos modelos de chat e agentes.
- Google Sheets OAuth2, usado para carregar a base de conhecimento.

As credenciais ficam configuradas dentro do n8n e não devem ser versionadas no repositório.

## Como conseguir uma chave da OpenAI API

Documentação oficial:

- [OpenAI API Quickstart](https://platform.openai.com/docs/quickstart)
- [OpenAI API Keys](https://platform.openai.com/api-keys)

Passo a passo:

1. Acesse [https://platform.openai.com](https://platform.openai.com).
2. Faça login ou crie uma conta.
3. Abra a área de API keys: [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys).
4. Selecione o projeto correto ou crie um novo projeto para o QA Copilot.
5. Clique em `Create new secret key`.
6. Dê um nome identificável para a chave, por exemplo `qa-copilot-n8n`.
7. Copie a chave gerada e guarde em um local seguro.
8. Configure billing, limites de uso e permissões do projeto conforme a política da sua conta.
9. No n8n, crie uma credencial do tipo `OpenAI API`.
10. Cole a chave no campo da credencial e salve.
11. Volte ao workflow e selecione essa credencial em todos os nodes `OpenAI Chat Model`.

Cuidados importantes:

- A chave aparece apenas uma vez no momento da criação. Se perder, gere outra.
- Nunca coloque a chave no GitHub, README, CSV, workflow exportado ou prints.
- Use uma chave por ambiente quando possível, por exemplo `dev`, `homolog` e `prod`.
- Revogue imediatamente a chave se houver suspeita de vazamento.
- Em scripts locais, prefira variável de ambiente `OPENAI_API_KEY` em vez de chave fixa no código.

## Configuração recomendada

Após importar o workflow, confira:

- Se todos os nodes `OpenAI Chat Model` apontam para a credencial correta.
- Se o modelo configurado está disponível na sua conta.
- Se o Google Sheets possui a aba `knowledge` com as colunas esperadas.
- Se o campo `sessionId` está chegando pelo chat para manter memória por sessão.
- Se o roteador retorna exatamente uma das intenções aceitas pelo `Switch`.

## Ferramentas que podem ajudar no processo

O QA Copilot pode ser evoluído com integrações no n8n para buscar contexto, registrar evidências, abrir tarefas e publicar documentação automaticamente.

| Ferramenta | Como pode ajudar |
| --- | --- |
| Jira | Buscar user stories, critérios de aceite, bugs e épicos; criar subtarefas de QA; anexar evidências; atualizar status e comentários. |
| Xray ou Zephyr | Criar casos de teste, cenários Gherkin, planos de teste, execuções e rastreabilidade entre requisito, teste e defeito. |
| Confluence | Consultar documentação funcional, regras de negócio, manuais, atas de refinamento e publicar planos ou relatórios de teste. |
| Google Sheets | Manter a base de conhecimento, massas de teste, matriz de risco, checklist de regressão e dados de apoio para o workflow. |
| Google Drive ou OneDrive | Armazenar evidências, prints, relatórios, coleções, arquivos de massa e documentos gerados pelo QA Copilot. |
| Slack ou Microsoft Teams | Enviar resumo de análise, riscos, status de execução, alertas de falha e links para evidências ou issues. |
| GitHub ou GitLab | Ler pull requests, issues e pipelines; comentar achados de QA; acompanhar status de CI/CD e versionar coleções ou scripts. |
| Azure DevOps | Integrar work items, test plans, pipelines, bugs e rastreabilidade em times que usam ecossistema Microsoft. |
| Postman ou Bruno API | Organizar coleções de API, ambientes, variáveis, exemplos de payload e validações manuais ou automatizadas. |
| OpenAPI ou Swagger | Usar contratos de API como fonte para gerar cenários, validar payloads, comparar schema e identificar mudanças quebráveis. |
| Playwright | Automatizar testes end-to-end de UI, gerar evidências de execução e integrar cenários críticos ao pipeline. |
| Rest Assured | Automatizar testes de API em Java, validando status, headers, schema, payloads, autenticação e contratos. |
| K6 ou JMeter | Apoiar testes de performance, carga, stress, spike e coleta de métricas como latência, throughput e taxa de erro. |
| Grafana, Datadog ou New Relic | Consultar métricas, logs, traces e dashboards para enriquecer análise de falhas e evidências de performance. |
| ServiceNow | Registrar incidentes, problemas, mudanças e evidências quando o processo de QA estiver ligado a governança corporativa. |

Sugestões de automações úteis:

- Criar casos de teste no Jira/Xray a partir de uma user story.
- Gerar página no Confluence com plano de teste, riscos e critérios de aceite revisados.
- Consultar a base no Google Sheets antes de responder perguntas de QA.
- Enviar resumo no Slack ou Teams quando uma análise crítica for concluída.
- Anexar evidências no Jira ou Google Drive após execução de teste.
- Ler contrato OpenAPI e gerar cenários positivos, negativos e de segurança.
- Consultar logs e métricas em ferramentas de observabilidade para investigar falhas.

## Exemplos de perguntas

- `Crie cenários Gherkin para transferência PIX.`
- `Como testar autenticação JWT em uma API financeira?`
- `Revise esta user story e sugira critérios de aceite.`
- `Quais testes LGPD devo considerar para logs de auditoria?`
- `Monte uma estratégia de performance para consulta de saldo.`
- `Sugira uma arquitetura de automação com Playwright e Rest Assured.`

## Cuidados de segurança

- Não envie chaves de API, tokens, senhas ou credenciais para o GitHub.
- Não use CPF real, token real, senha real ou dados bancários reais como massa de teste.
- Revise respostas antes de usar em documentação oficial ou evidências regulatórias.
- Mantenha a base de conhecimento sem dados sensíveis.

## Arquivos principais

- [workflows/qa-copilot.json](workflows/qa-copilot.json): workflow importável no n8n.
- [data/qa-knowledge-base.csv](data/qa-knowledge-base.csv): base de conhecimento local de referência.
