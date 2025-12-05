# API Progressão de Alunos de Música

## Descrição
API Rest para acompanhamento da progressão de alunos de música. Permite registro e autenticação de instrutores e alunos, cadastro de lições, registro de progresso e consulta de progresso dos alunos.

## Funcionalidades
- Registro e login de instrutor
- Registro e login de aluno
- Cadastro de lições
- Registro de lições realizadas pelo aluno
- Consulta de progresso do aluno
- Autenticação via JWT
- Documentação Swagger

## Estrutura do Projeto
- `routes/` - Rotas da API
- `controllers/` - Lógica de controle das rotas
- `service/` - Regras de negócio e manipulação dos dados
- `model/` - Modelos de dados
- `resources/` - Documentação Swagger
- `middleware/` - Middleware de autenticação

## Banco de Dados
- Utiliza armazenamento em memória (não persiste dados após reiniciar o servidor)

## Autenticação
- Instrutores: acesso total às funcionalidades
- Alunos: acesso apenas à consulta de progresso
- Autenticação via JWT, middleware controla acesso por tipo de usuário

## Documentação Swagger
- Disponível em `/swagger` após iniciar o servidor

## Como executar
1. Instale as dependências:
   ```bash
   npm install express jsonwebtoken swagger-ui-express js-yaml
   ```
2. Inicie o servidor:
   ```bash
   node app.js
   ```
3. Acesse a documentação Swagger em [http://localhost:3000/swagger](http://localhost:3000/swagger)

## Endpoints principais
Consulte o arquivo `resources/swagger.yaml` para detalhes dos endpoints e modelos de dados.

## Testes de Performance

### K6
Scripts de teste de carga usando K6 localizados em `testes/k6/`.

#### Pré-requisitos:
- [K6 instalado](https://k6.io/docs/getting-started/installation/)
- API rodando em `http://localhost:3000`

#### Executar teste básico:
```bash
k6 run testes/k6/primeiro-script.js
```

#### Executar com dashboard web em tempo real:
```bash
# PowerShell
$env:K6_WEB_DASHBOARD="true"; k6 run testes/k6/primeiro-script.js

# Git Bash / Linux / macOS
K6_WEB_DASHBOARD=true k6 run testes/k6/primeiro-script.js
```

O dashboard estará disponível em: `http://localhost:5665`

#### Executar com período personalizado de atualização:
```bash
# PowerShell (recomendado para testes de 30-60s)
$env:K6_WEB_DASHBOARD="true"; $env:K6_WEB_DASHBOARD_PERIOD="1s"; k6 run testes/k6/primeiro-script.js

# Git Bash / Linux / macOS
K6_WEB_DASHBOARD=true K6_WEB_DASHBOARD_PERIOD="2s" k6 run testes/k6/primeiro-script.js
```

#### Gerar relatórios em JSON:
```bash
# PowerShell
$env:K6_WEB_DASHBOARD="true"; k6 run --out json=results.json --summary-export=summary.json testes/k6/primeiro-script.js

# Git Bash / Linux / macOS
K6_WEB_DASHBOARD=true k6 run --out json=results.json --summary-export=summary.json testes/k6/primeiro-script.js
```

#### Ver estatísticas detalhadas:
```bash
k6 run --summary-trend-stats="avg,min,med,max,p(90),p(95),p(99)" testes/k6/primeiro-script.js
```

#### Cenários de teste K6:
- **Teste básico:** Login de instrutor + Criação de lição
- **Configuração:** 10 VUs por 20 segundos
- **Métricas:** Response time, throughput, taxa de erro
- **Relatórios:** Dashboard web, JSON, summary personalizado

### JMeter
Testes de performance usando Apache JMeter localizados em `testes/jmeter/`.

#### Arquivos disponíveis:
- `Teste de Performance de Fluxo de Instrutores.jmx`
- `Teste de performance de registro de aluno com login e consulta de progresso.jmx`
- `test-data/students.csv` - 50 estudantes para parametrização

#### Executar via linha de comando:
```bash
# Command Prompt / PowerShell
C:\apache-jmeter-5.6.3\bin\jmeter.bat -n -t "testes/jmeter/Teste de Performance de Fluxo de Instrutores.jmx" -l "report-jmeter/log-execucao.jtl"

# Git Bash
/c/apache-jmeter-5.6.3/bin/jmeter.sh -n -t "testes/jmeter/Teste de Performance de Fluxo de Instrutores.jmx" -l "report-jmeter/log-execucao.jtl"
```

#### Gerar relatório HTML:
```bash
C:\apache-jmeter-5.6.3\bin\jmeter.bat -g report-jmeter/log-execucao.jtl -o report-jmeter/html-report
```

## Observações
- Este projeto é apenas para fins de demonstração e não deve ser usado em produção sem persistência de dados e segurança adequada.
