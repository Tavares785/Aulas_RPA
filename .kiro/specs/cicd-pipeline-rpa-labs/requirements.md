# Requirements Document

## Introduction

Este documento especifica os requisitos para a criação e melhoria dos pipelines de CI/CD do projeto de ensino de RPA (Robotic Process Automation) da UNIFAAT. O projeto contém 14 laboratórios práticos (AULA01 a AULA14) com scripts Python de complexidades e dependências distintas. O objetivo é garantir que cada grupo de aulas possua um pipeline adequado ao seu perfil técnico — cobrindo desde validação de sintaxe e testes unitários até execução de bots web em ambiente headless, geração de artefatos e deploy do projeto final — transformando o repositório em um modelo de boas práticas de engenharia de software para os alunos.

---

## Glossary

- **Pipeline**: Conjunto automatizado de etapas executadas pelo GitHub Actions em resposta a eventos do repositório.
- **CI (Continuous Integration)**: Prática de validar código automaticamente a cada push ou pull request.
- **CD (Continuous Delivery/Deployment)**: Prática de entregar ou executar o produto automaticamente após a validação.
- **GitHub_Actions**: Plataforma de automação de workflows integrada ao GitHub, utilizada para todos os pipelines.
- **Job**: Unidade de trabalho dentro de um workflow do GitHub Actions, composta por steps sequenciais.
- **Step**: Etapa individual dentro de um job (ex: instalar dependências, executar testes).
- **Matrix**: Estratégia do GitHub Actions que executa o mesmo job em múltiplas configurações em paralelo.
- **Artefato**: Arquivo gerado durante a execução do pipeline e disponibilizado para download na interface do GitHub.
- **Xvfb**: Servidor de display virtual para Linux, utilizado para executar aplicações gráficas em ambiente sem tela.
- **Headless**: Modo de execução de navegador sem interface gráfica visível.
- **Secret**: Variável de ambiente criptografada armazenada nas configurações do repositório GitHub.
- **Flake8**: Ferramenta de análise estática de código Python para verificar conformidade com PEP8.
- **Pytest**: Framework de testes para Python.
- **Cache**: Mecanismo de armazenamento de dependências instaladas para acelerar execuções futuras do pipeline.
- **Workflow**: Arquivo YAML na pasta `.github/workflows/` que define um pipeline do GitHub Actions.
- **Trigger**: Evento que inicia a execução de um workflow (ex: push, pull_request, schedule).
- **Runner**: Máquina virtual provisionada pelo GitHub para executar os jobs (padrão: `ubuntu-latest`).
- **Bot_Headless**: Script de automação RPA executado sem interface gráfica em ambiente CI.
- **Grupo_Iniciais**: Conjunto de labs AULA01, AULA02, AULA03 (fundamentos de Python para RPA).
- **Grupo_Resiliencia**: Conjunto de labs AULA04 e AULA05 (tratamento de erros, logs e análise de processos).
- **Grupo_Desktop**: Conjunto de labs AULA06 e AULA07 (automação de interface gráfica desktop com PyAutoGUI).
- **Grupo_Web**: Conjunto de labs AULA08 e AULA09 (automação web com Selenium).
- **Grupo_Dados**: Conjunto de labs AULA10 e AULA11 (extração de PDF, Excel e web scraping).
- **Grupo_Integracao**: Conjunto de labs AULA12 e AULA13 (APIs REST, SMTP e arquitetura assíncrona).
- **Grupo_Arquitetura**: Lab AULA14 (estrutura de repositório, README e boas práticas).
- **Projeto_Final**: Pipeline de CD que executa o bot de produção com secrets e agendamento.

---

## Requirements

### Requirement 1: Validação de Qualidade de Código para Labs Iniciais (AULA01–AULA03)

**User Story:** Como professor, quero que os scripts dos labs iniciais (fundamentos Python) sejam validados automaticamente a cada push, para que os alunos recebam feedback imediato sobre erros de sintaxe e boas práticas PEP8.

#### Acceptance Criteria

1. WHEN um push ou pull request é enviado para a branch `main`, THE GitHub_Actions SHALL executar o workflow `ci_aulas_iniciais.yml` em um runner `ubuntu-latest` com Python 3.10.
2. WHEN o workflow for iniciado, THE GitHub_Actions SHALL realizar o checkout do repositório e configurar Python 3.10 e Python 3.11 em paralelo usando Matrix com `fail-fast: false`, garantindo que ambas as versões sempre executem independentemente de falhas.
3. THE GitHub_Actions SHALL utilizar cache de dependências pip com chave baseada no sistema operacional, versão Python e hash do `requirements.txt`.
4. THE GitHub_Actions SHALL executar o `flake8` para detectar erros críticos de sintaxe (`E9`, `F63`, `F7`, `F82`) em todos os arquivos `.py`, excluindo `.venv/`.
5. THE GitHub_Actions SHALL executar o `flake8` para avaliar conformidade PEP8, com complexidade máxima de 10, comprimento máximo de linha de 127 caracteres, e excluindo o diretório `.venv/`.
6. WHEN arquivos de teste (`test_*.py` ou `tests/**/*.py`) forem detectados no repositório, THE GitHub_Actions SHALL executar o `pytest` exibindo apenas a contagem de testes aprovados, reprovados e erros, sem detalhamento individual de cada teste.
7. IF nenhum arquivo de teste for encontrado, THEN THE GitHub_Actions SHALL pular a etapa de pytest sem falhar o pipeline.
8. IF o `flake8` detectar erros críticos de sintaxe, THEN THE GitHub_Actions SHALL falhar o job imediatamente e exibir a fonte do erro.
9. IF o arquivo `requirements.txt` não existir no repositório, THEN THE GitHub_Actions SHALL pular a etapa de instalação de dependências sem falhar o pipeline.

---

### Requirement 2: Testes Unitários com Matriz de Versões Python (AULA04)

**User Story:** Como professor, quero que o lab de resiliência (AULA04) execute testes unitários com cobertura de código em múltiplas versões Python, para que os alunos entendam compatibilidade e qualidade de testes.

#### Acceptance Criteria

1. WHEN um push ou pull request (eventos `opened`, `synchronize` ou `reopened`) é enviado para a branch `main`, THE GitHub_Actions SHALL executar o workflow `ci_aula_04.yml`.
2. WHEN o workflow for iniciado, THE GitHub_Actions SHALL configurar a estratégia Matrix com Python 3.10 e Python 3.11 e `fail-fast: false`, executando os jobs em paralelo sem cancelar ao primeiro falhar.
3. WHEN cada job da Matrix for inicializado, THE GitHub_Actions SHALL instalar `pytest` e `pytest-cov` antes de executar os testes.
4. WHEN os testes forem executados, THE GitHub_Actions SHALL gerar relatório de cobertura de código no terminal com linhas não cobertas indicadas (`--cov-report=term-missing`) e falhar o job se a cobertura total for inferior a 80%.
5. THE GitHub_Actions SHALL ignorar o diretório `.venv/` durante a execução do pytest.
6. IF o `requirements.txt` existir no repositório, THEN THE GitHub_Actions SHALL instalar todas as dependências listadas antes de executar os testes.

---

### Requirement 3: Execução de Bots Desktop em Display Virtual (AULA06–AULA07)

**User Story:** Como professor, quero que os labs de automação desktop (PyAutoGUI) sejam validados em ambiente CI com display virtual, para demonstrar aos alunos que bots de interface gráfica podem ser executados em servidores Linux sem monitor.

#### Acceptance Criteria

1. WHEN um push ou pull request é enviado para a branch `main`, THE GitHub_Actions SHALL executar o workflow `ci_aula_06_09.yml` em um runner `ubuntu-latest` com Python 3.10.
2. WHEN o job for inicializado, THE GitHub_Actions SHALL instalar `xvfb`, `libxi6`, `libgconf-2-4` e `google-chrome-stable` via `apt-get` nesta ordem, antes de qualquer instalação Python.
3. THE GitHub_Actions SHALL instalar as bibliotecas Python `selenium`, `webdriver-manager`, `pyautogui`, `pytest` e `pytest-xvfb`.
4. WHEN o `pytest` for executado, THE GitHub_Actions SHALL envolver a execução com `xvfb-run` configurando resolução `1920x1080x24`.
5. IF um script de automação falhar durante a execução em display virtual, THEN THE GitHub_Actions SHALL registrar o erro no log do step correspondente e o step deverá completar com exit code 0.
6. IF o `requirements.txt` existir no repositório, THEN THE GitHub_Actions SHALL instalar todas as dependências listadas.

---

### Requirement 4: Validação de Bots Web com Selenium (AULA08–AULA09)

**User Story:** Como professor, quero que os labs de automação web (Selenium) sejam validados em modo headless, para que os alunos vejam como bots web funcionam em ambiente de produção sem interface gráfica.

#### Acceptance Criteria

1. WHEN um push ou pull request é enviado para a branch `main`, THE GitHub_Actions SHALL executar o pipeline de validação dos labs AULA08 e AULA09.
2. THE GitHub_Actions SHALL instalar o Google Chrome e o ChromeDriver compatível via `webdriver-manager`.
3. THE GitHub_Actions SHALL configurar o Chrome com os argumentos `--headless`, `--no-sandbox` e `--disable-dev-shm-usage` para execução sem interface gráfica e compatibilidade com o ambiente Runner do GitHub Actions.
4. WHEN o Chrome e o ChromeDriver forem inicializados com sucesso, THE GitHub_Actions SHALL prosseguir com a execução dos scripts Selenium; IF a inicialização falhar, THEN THE GitHub_Actions SHALL registrar o erro no log do step e falhar o job imediatamente.
5. IF a validação de elementos DOM falhar por timeout de espera, THEN THE GitHub_Actions SHALL registrar no log do step correspondente a URL acessada, o seletor aguardado e o tempo decorrido antes do timeout.
6. THE GitHub_Actions SHALL utilizar Python 3.10 para execução dos scripts Selenium nos labs AULA08 e AULA09.

---

### Requirement 5: Extração de Dados e Publicação de Artefatos (AULA10–AULA11)

**User Story:** Como professor, quero que os labs de extração de dados (PDF e web scraping) gerem artefatos downloadáveis no GitHub Actions, para que os alunos entendam o ciclo completo de extração, transformação e entrega de dados em CI/CD.

#### Acceptance Criteria

1. WHEN um push, pull request ou disparo manual (`workflow_dispatch`) ocorrer, THE GitHub_Actions SHALL executar o workflow `pipeline_aulas_10_11.yml`.
2. WHEN o job for inicializado, THE GitHub_Actions SHALL instalar as bibliotecas `pandas`, `openpyxl`, `pdfplumber`, `requests` e `beautifulsoup4`.
3. WHEN o script `leitor_faturas_pdf.py` for executado, THE GitHub_Actions SHALL tentar gerar o arquivo `relatorio_faturas.xlsx`.
4. THE GitHub_Actions SHALL publicar o arquivo `relatorio_faturas.xlsx` como artefato com nome `relatorio-faturas-excel`, retenção de 7 dias, e sem falhar caso o arquivo não exista (`if-no-files-found: ignore`).
5. WHEN o script `scraper_noticias.py` for executado, THE GitHub_Actions SHALL tentar gerar o arquivo `noticias.csv` como saída.
6. IF qualquer script de extração falhar, THEN THE GitHub_Actions SHALL registrar o erro no log sem interromper a etapa de publicação de artefatos.
7. THE GitHub_Actions SHALL publicar o arquivo `noticias.csv` como artefato com nome `relatorio-noticias-csv`, retenção de 7 dias, e sem falhar caso o arquivo não exista (`if-no-files-found: ignore`).

---

### Requirement 6: Validação de Integração com APIs e Arquitetura Assíncrona (AULA12–AULA13)

**User Story:** Como professor, quero que os labs de integração com APIs REST e processamento assíncrono sejam validados em CI, para que os alunos entendam como testar integrações de rede em pipelines automatizados.

#### Acceptance Criteria

1. WHEN um push ou pull request é enviado para a branch `main`, THE GitHub_Actions SHALL executar o pipeline de validação dos labs AULA12 e AULA13.
2. WHEN o job for inicializado, THE GitHub_Actions SHALL instalar as bibliotecas `requests` e `tenacity`, além de todas as dependências declaradas no `requirements.txt` (exceto `smtplib`, que faz parte da biblioteca padrão Python e não requer instalação separada).
3. WHEN o job for configurado para execução do script `bot_cotacao_alerta.py`, THE GitHub_Actions SHALL definir a variável de ambiente `MOCK_EMAIL=true` antes da execução para evitar envio real de e-mails.
4. WHEN o script `bot_faturamento_avancado.py` for executado, THE GitHub_Actions SHALL validar que o arquivo de log `app_rpa.log` é criado e contém ao menos uma linha de texto como evidência de execução bem-sucedida.
5. IF uma chamada de API externa falhar por instabilidade de rede, THEN THE GitHub_Actions SHALL registrar a mensagem de erro no log do step correspondente e o step deverá completar com exit code 0.

---

### Requirement 7: Validação de Estrutura de Repositório e Boas Práticas (AULA14)

**User Story:** Como professor, quero que o lab de arquitetura de repositório (AULA14) seja validado automaticamente, para que os alunos recebam feedback sobre a presença dos arquivos obrigatórios (`.gitignore`, `requirements.txt`, `README.md`).

#### Acceptance Criteria

1. WHEN um push ou pull request é enviado para a branch `main`, THE GitHub_Actions SHALL executar o pipeline de validação do lab AULA14.
2. WHEN o pipeline for executado, THE GitHub_Actions SHALL verificar a presença do arquivo `requirements.txt` na raiz do repositório e, caso esteja ausente, registrar uma linha no log contendo a palavra "AVISO" e o nome do arquivo.
3. WHEN o pipeline for executado, THE GitHub_Actions SHALL verificar a presença do arquivo `.gitignore` na raiz do repositório e, caso esteja ausente, registrar uma linha no log contendo a palavra "AVISO" e o nome do arquivo.
4. WHEN o pipeline for executado, THE GitHub_Actions SHALL verificar a presença do arquivo `README.md` na raiz do repositório e, caso esteja ausente, registrar uma linha no log contendo a palavra "AVISO" e o nome do arquivo.
5. WHEN o arquivo `.gitignore` estiver presente, THE GitHub_Actions SHALL validar que existe uma linha com o texto exato `.venv/` e uma linha com o texto exato `*.log` no arquivo.
6. IF algum arquivo obrigatório estiver ausente, THEN THE GitHub_Actions SHALL completar o job com exit code 0, preservando o caráter pedagógico da validação.
7. WHEN o job for concluído, THE GitHub_Actions SHALL exibir um resumo consolidado no log listando quais arquivos obrigatórios estavam presentes e quais estavam ausentes.

---

### Requirement 8: Pipeline de CD com Secrets e Agendamento para Projeto Final

**User Story:** Como professor, quero que o pipeline de CD do projeto final execute o bot de produção com secrets criptografados e agendamento automático, para demonstrar aos alunos como bots unattended são operados em produção.

#### Acceptance Criteria

1. WHEN um push ou pull request (eventos `opened`, `synchronize` ou `reopened`) é enviado para a branch `main`, THE GitHub_Actions SHALL executar o workflow `cd_projeto_final.yml`.
2. WHEN o agendamento diário às 00:00 UTC (cron `0 0 * * *`) for atingido, THE GitHub_Actions SHALL executar o workflow `cd_projeto_final.yml` automaticamente.
3. WHEN o disparo manual (`workflow_dispatch`) for acionado, THE GitHub_Actions SHALL executar o workflow `cd_projeto_final.yml`.
4. THE GitHub_Actions SHALL injetar os secrets `SMTP_EMAIL_USER`, `SMTP_EMAIL_PASS` e `URL_SISTEMA_PROD` como variáveis de ambiente `EMAIL_REMETENTE`, `SENHA_APLICACAO` e `URL_SISTEMA_LEGADO` respectivamente.
5. IF o arquivo `main.py` for encontrado na raiz do repositório, THEN THE GitHub_Actions SHALL executar o bot com as variáveis de ambiente injetadas.
6. IF o arquivo `main.py` não for encontrado, THEN THE GitHub_Actions SHALL exibir mensagem informativa `"Arquivo main.py não encontrado."` sem falhar o pipeline.
7. IF o arquivo `requirements.txt` existir no repositório, THEN THE GitHub_Actions SHALL instalar todas as dependências listadas antes de executar o bot de produção; IF `requirements.txt` não existir, THEN THE GitHub_Actions SHALL pular a etapa de instalação sem falhar o job.

---

### Requirement 9: Padronização e Consistência dos Workflows

**User Story:** Como professor, quero que todos os workflows sigam um padrão consistente de estrutura e nomenclatura, para que o repositório sirva como modelo de boas práticas de CI/CD para os alunos.

#### Acceptance Criteria

1. THE GitHub_Actions SHALL utilizar `actions/checkout@v4` como step de checkout em todos os workflows.
2. THE GitHub_Actions SHALL utilizar `actions/setup-python@v5` para configuração do ambiente Python em todos os workflows.
3. IF um workflow possuir um arquivo `requirements.txt` ou usar estratégia Matrix com múltiplas versões Python, THEN THE GitHub_Actions SHALL utilizar `actions/cache@v4` com chave estruturada como `${{ runner.os }}-pip-${{ matrix.python-version }}-${{ hashFiles('requirements.txt') }}`.
4. IF um workflow gerar arquivos persistidos fora do runner (ex: relatórios, logs, exports), THEN THE GitHub_Actions SHALL utilizar `actions/upload-artifact@v4` com `retention-days` entre 1 e 30 dias.
5. WHEN um novo workflow for criado para labs não cobertos, THE GitHub_Actions SHALL seguir a convenção de nomenclatura `ci_aula_XX.yml` ou `ci_aulas_XX_YY.yml`, onde `XX` e `YY` são números de dois dígitos com zero à esquerda.
6. THE GitHub_Actions SHALL utilizar `ubuntu-latest` como runner padrão em todos os workflows; IF outro sistema operacional for necessário, THEN a justificativa deverá ser documentada via comentário no próprio arquivo de workflow.
7. WHEN o workflow `pipeline_aulas_10_11.yml` for renomeado, THE GitHub_Actions SHALL adotar o nome `ci_aulas_10_11.yml` para conformidade com a convenção de nomenclatura definida no critério 5.

---

### Requirement 10: Cobertura Completa — Workflows para Labs Sem Pipeline

**User Story:** Como professor, quero que os labs atualmente sem pipeline dedicado (AULA02, AULA03, AULA05, AULA12, AULA13, AULA14) sejam cobertos por workflows de CI, para que todos os 14 labs tenham validação automatizada.

#### Acceptance Criteria

1. THE GitHub_Actions SHALL cobrir o lab AULA01 com validação de linting (`flake8`) e testes (`pytest`) no workflow `ci_aulas_iniciais.yml`; para os labs AULA02 e AULA03, que não possuem arquivos Python, THE GitHub_Actions SHALL validar a presença e formato dos arquivos Markdown obrigatórios (`lab-XX.md` e `lab-XX_resp.md`).
2. THE GitHub_Actions SHALL cobrir o lab AULA05 com validação de presença do arquivo `AVALIACAO_PROCESSO.md` em um step dedicado no workflow `ci_documentos.yml`; IF o arquivo estiver ausente, THEN THE GitHub_Actions SHALL registrar uma linha no log contendo "AVISO" e o nome do arquivo.
3. THE GitHub_Actions SHALL cobrir os labs AULA12 e AULA13 com um novo workflow dedicado que valide a sintaxe Python dos arquivos `bot_cotacao_alerta.py` e `bot_faturamento_avancado.py`.
4. THE GitHub_Actions SHALL cobrir o lab AULA14 com um novo workflow dedicado que valide a presença dos arquivos `README.md`, `requirements.txt` e `.gitignore` na raiz do repositório.
5. WHEN todos os workflows forem executados para um push na branch `main`, THE GitHub_Actions SHALL ter cobertura de validação para os 14 labs: AULA01–AULA14, sem nenhum lab sem step de validação associado.
6. IF um lab não possuir scripts Python testáveis, THEN THE GitHub_Actions SHALL validar a presença dos arquivos Markdown obrigatórios (`lab-XX.md` e `lab-XX_resp.md`) e verificar que cada arquivo é não-vazio e contém ao menos um título Markdown (linha iniciada com `#`).
