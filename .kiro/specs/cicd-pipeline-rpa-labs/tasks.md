# Implementation Plan: CI/CD Pipeline para RPA Labs (UNIFAAT)

## Overview

Implementação incremental dos 8 workflows GitHub Actions e dos testes de propriedade que validam a estrutura YAML. O plano segue a ordem natural de dependências: primeiro melhorias nos workflows existentes, depois criação dos novos workflows e, por último, os testes que validam o conjunto completo.

## Tasks

- [x] 1. Melhorar `ci_aulas_iniciais.yml` (workflows existente — AULA01–03)
  - [x] 1.1 Adicionar `fail-fast: false` na estratégia Matrix e `--exclude=.venv` nos comandos flake8
    - Abrir `.github/workflows/ci_aulas_iniciais.yml`
    - Inserir `fail-fast: false` na chave `strategy`
    - Adicionar `--exclude=.venv` em ambos os steps do flake8 (erros críticos e PEP8)
    - Ajustar a detecção de testes para usar `find . -name "test_*.py" -not -path "./.venv/*"` em vez de glob
    - _Requirements: 1.2, 1.4, 1.5, 9.1, 9.2, 9.3_

  - [x] 1.2 Escrever testes unitários para `ci_aulas_iniciais.yml`
    - Criar `tests/test_ci_aulas_iniciais.py`
    - Verificar presença de `fail-fast: false`
    - Verificar flags `--exclude=.venv` nos dois steps flake8
    - Verificar chave de cache estruturada corretamente
    - _Requirements: 1.2, 1.4, 1.5_

- [x] 2. Melhorar `ci_aula_04.yml` (workflow existente — AULA04)
  - [x] 2.1 Adicionar `fail-fast: false`, `cache@v4` e flag `--cov-fail-under=80`
    - Abrir `.github/workflows/ci_aula_04.yml`
    - Inserir `fail-fast: false` na chave `strategy`
    - Adicionar step `cache@v4` com chave `${{ runner.os }}-pip-${{ matrix.python-version }}-${{ hashFiles('requirements.txt') }}`
    - Adicionar `--cov-fail-under=80` ao comando pytest
    - _Requirements: 2.2, 2.4, 2.5, 9.3_

  - [x] 2.2 Escrever testes unitários para `ci_aula_04.yml`
    - Criar `tests/test_ci_aula_04.py`
    - Verificar `fail-fast: false`
    - Verificar step de cache com chave correta
    - Verificar flag `--cov-fail-under=80` no comando pytest
    - _Requirements: 2.2, 2.4_

- [ ] 3. Renomear e melhorar `pipeline_aulas_10_11.yml` → `ci_aulas_10_11.yml` (AULA10–11)
  - [x] 3.1 Criar `ci_aulas_10_11.yml` com steps adicionais para `scraper_noticias.py` e segundo artefato
    - Criar `.github/workflows/ci_aulas_10_11.yml` com o conteúdo do pipeline atual
    - Adicionar step `python AULA11/scraper_noticias.py || true` após o step de faturas
    - Adicionar step `upload-artifact@v4` para `noticias.csv` com nome `relatorio-noticias-csv`, `retention-days: 7`, `if-no-files-found: ignore`
    - _Requirements: 5.1, 5.5, 5.6, 5.7, 9.5, 9.7_

  - [x] 3.2 Deletar o arquivo `pipeline_aulas_10_11.yml` após a criação do novo
    - Remover `.github/workflows/pipeline_aulas_10_11.yml` do repositório
    - _Requirements: 9.7_

  - [~] 3.3 Escrever testes unitários para `ci_aulas_10_11.yml`
    - Criar `tests/test_ci_aulas_10_11.py`
    - Verificar que `pipeline_aulas_10_11.yml` não existe mais
    - Verificar que `ci_aulas_10_11.yml` existe
    - Verificar presença do step de scraper e dos dois artefatos
    - Verificar `retention-days: 7` e `if-no-files-found: ignore` em ambos os artefatos
    - _Requirements: 5.4, 5.7, 9.7_

- [x] 4. Criar `ci_documentos.yml` (novo — AULA02, AULA03, AULA05)
  - [x] 4.1 Implementar workflow de validação de Markdown obrigatório
    - Criar `.github/workflows/ci_documentos.yml`
    - Definir triggers `push` e `pull_request` para branch `main`
    - Usar `ubuntu-latest`, `checkout@v4` (sem Python — só bash)
    - Adicionar step para verificar `AULA02/lab-02.md` e `AULA02/lab-02_resp.md`: não-vazios e contêm `#`
    - Adicionar step para verificar `AULA03/lab-03.md` e `AULA03/lab-03_resp.md`: não-vazios e contêm `#`
    - Adicionar step para verificar `AULA05/AVALIACAO_PROCESSO.md`: presença e conteúdo mínimo com `#`
    - Adicionar step de resumo consolidado listando arquivos válidos e com AVISO
    - Garantir `exit 0` ao final para caráter pedagógico
    - _Requirements: 10.1, 10.2, 10.6_

  - [x] 4.2 Escrever testes unitários para `ci_documentos.yml`
    - Criar `tests/test_ci_documentos.py`
    - Verificar que o workflow usa `checkout@v4`
    - Verificar que todos os arquivos Markdown obrigatórios têm steps dedicados
    - Verificar ausência de `setup-python` (workflow usa somente bash)
    - _Requirements: 10.1, 10.2_

- [x] 5. Criar `ci_aulas_12_13.yml` (novo — AULA12, AULA13)
  - [x] 5.1 Implementar workflow de validação de sintaxe e execução controlada dos bots de integração
    - Criar `.github/workflows/ci_aulas_12_13.yml`
    - Definir triggers `push` e `pull_request` para branch `main`
    - Usar `ubuntu-latest`, `checkout@v4`, `setup-python@v5` com Python 3.10
    - Adicionar step para instalar `requests`, `tenacity` e `requirements.txt` condicional
    - Adicionar step `python -m py_compile AULA12/bot_cotacao_alerta.py` (bloqueante — falha imediata em erro de sintaxe)
    - Adicionar step `python -m py_compile AULA13/bot_faturamento_avancado.py` (bloqueante)
    - Adicionar step para executar `AULA12/bot_cotacao_alerta.py` com `env: MOCK_EMAIL: "true"` e `|| true`
    - Adicionar step para executar `AULA13/bot_faturamento_avancado.py` com `|| true`
    - Adicionar step para verificar criação de `app_rpa.log` com ao menos uma linha (falha se ausente)
    - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5, 10.3_

  - [x] 5.2 Escrever testes unitários para `ci_aulas_12_13.yml`
    - Criar `tests/test_ci_aulas_12_13.py`
    - Verificar presença dos steps de `py_compile` para ambos os scripts
    - Verificar que `MOCK_EMAIL: "true"` está definido no step de execução do bot de cotação
    - Verificar que o step de verificação do `app_rpa.log` está presente
    - _Requirements: 6.3, 6.4_

- [x] 6. Criar `ci_aula_14.yml` (novo — AULA14)
  - [x] 6.1 Implementar workflow de validação de estrutura de repositório
    - Criar `.github/workflows/ci_aula_14.yml`
    - Definir triggers `push` e `pull_request` para branch `main`
    - Usar `ubuntu-latest`, `checkout@v4` (apenas bash)
    - Adicionar step para verificar presença de `requirements.txt` com `AVISO` se ausente
    - Adicionar step para verificar presença de `.gitignore` com `AVISO` se ausente; se presente, validar linhas `.venv/` e `*.log`
    - Adicionar step para verificar presença de `README.md` com `AVISO` se ausente
    - Adicionar step de resumo consolidado listando presentes vs ausentes
    - Garantir `exit 0` explícito no final do job para caráter pedagógico
    - _Requirements: 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 10.4_

  - [ ] 6.2 Escrever testes unitários para `ci_aula_14.yml`
    - Criar `tests/test_ci_aula_14.py`
    - Verificar que os três arquivos obrigatórios (`requirements.txt`, `.gitignore`, `README.md`) têm steps dedicados
    - Verificar que o script bash emite `AVISO` para arquivos ausentes
    - Verificar que o step final tem `exit 0` explícito
    - _Requirements: 7.2, 7.3, 7.4, 7.6_

- [~] 7. Checkpoint — Verificar consistência dos workflows criados e renomeados
  - Garantir que todos os 8 arquivos existem em `.github/workflows/`
  - Confirmar que `pipeline_aulas_10_11.yml` foi removido
  - Executar `python -m py_compile` em todos os scripts Python referenciados para garantir sintaxe válida
  - Garantir que todos os workflows usam `checkout@v4` e `setup-python@v5` (onde aplicável)
  - Perguntar ao usuário se há ajustes antes de prosseguir para os testes de propriedade

- [ ] 8. Criar infraestrutura de testes
  - [~] 8.1 Criar `tests/conftest.py` com helpers compartilhados para leitura e parsing de YAML
    - Criar o diretório `tests/` no repositório
    - Implementar `get_all_workflow_files()` — retorna lista de caminhos dos YAMLs em `.github/workflows/`
    - Implementar `get_matrix_workflows()` — retorna workflows com `strategy.matrix.python-version`
    - Implementar `get_upload_artifact_steps()` — retorna todos os steps `upload-artifact@v4` de todos os workflows
    - Implementar `load_workflow(path)` — faz parse do YAML e retorna dict
    - Implementar `is_valid_markdown(content: str) -> bool` — retorna `True` se não-vazio e contém linha com `#`
    - Implementar `check_required_files(has_req, has_git, has_readme) -> list[str]` — retorna lista de AVISOs
    - Implementar `detect_tests(file_names: list[str]) -> bool` — retorna `True` se algum arquivo começa com `test_`
    - _Requirements: 9.1, 9.2, 9.3, 9.4, 10.5_

  - [~] 8.2 Escrever teste de propriedade — Property 1: Padronização de actions em todos os workflows
    - Criar `tests/test_workflow_standards.py`
    - Importar `hypothesis`, `pytest` e helpers do `conftest.py`
    - Implementar `test_all_workflows_use_standard_actions` com `@given(st.sampled_from(get_all_workflow_files()))`
    - Verificar `actions/checkout@v4` e `actions/setup-python@v5` (onde Python é configurado) em cada workflow
    - Configurar `@settings(max_examples=100)`
    - **Property 1: Padronização de actions em todos os workflows**
    - **Validates: Requirements 9.1, 9.2**

  - [~] 8.3 Escrever teste de propriedade — Property 2: Cache obrigatório em workflows com Matrix
    - No mesmo arquivo `tests/test_workflow_standards.py`
    - Implementar `test_matrix_workflows_have_cache` com `@given(st.sampled_from(get_matrix_workflows()))`
    - Verificar presença de step `cache@v4` com chave contendo `runner.os`, `matrix.python-version` e `hashFiles`
    - Configurar `@settings(max_examples=100)`
    - **Property 2: Cache de pip obrigatório em workflows com Matrix**
    - **Validates: Requirements 9.3**

  - [~] 8.4 Escrever teste de propriedade — Property 3: Retenção válida em artefatos publicados
    - No mesmo arquivo `tests/test_workflow_standards.py`
    - Implementar `test_artifact_retention_in_valid_range` com `@given(st.sampled_from(get_upload_artifact_steps()))`
    - Verificar `1 <= retention_days <= 30` para cada step de upload-artifact encontrado
    - Configurar `@settings(max_examples=100)`
    - **Property 3: Retenção válida em artefatos publicados**
    - **Validates: Requirements 9.4**

- [ ] 9. Criar testes de propriedade para helpers de validação Python
  - [~] 9.1 Escrever teste de propriedade — Property 5: Validação de Markdown não-vazio com título
    - Criar `tests/test_markdown_validator.py`
    - Implementar `test_markdown_validation_logic` com `@given(st.text())`
    - Verificar que `is_valid_markdown(content)` retorna `True` se e somente se `len(content) > 0` e existe linha iniciada com `#`
    - Testar casos degenerados: string vazia, somente espaços, `#` no meio da linha
    - Configurar `@settings(max_examples=100)`
    - **Property 5: Validação de Markdown não-vazio com título**
    - **Validates: Requirements 10.6**

  - [~] 9.2 Escrever teste de propriedade — Property 6: Aviso proporcional à ausência de arquivos obrigatórios
    - Criar `tests/test_file_presence_checker.py`
    - Implementar `test_warning_proportional_to_absence` com `@given(st.booleans(), st.booleans(), st.booleans())`
    - Para cada combinação (has_requirements, has_gitignore, has_readme): verificar que `check_required_files()` emite `AVISO` exatamente para os arquivos ausentes e não emite para os presentes
    - Configurar `@settings(max_examples=100)`
    - **Property 6: Aviso de arquivo obrigatório é proporcional à ausência**
    - **Validates: Requirements 7.2, 7.3, 7.4**

  - [~] 9.3 Escrever teste de propriedade — Property 7: Detecção de testes coerente com estrutura do repositório
    - Criar `tests/test_test_detection.py`
    - Implementar `test_test_detection_coherence` com `@given(st.lists(st.text(alphabet=st.characters(whitelist_categories="Lu Ll Nd Pc"), min_size=0), max_size=20))`
    - Verificar que `detect_tests(file_names)` retorna `True` se e somente se ao menos um elemento começa com `test_` ou está em `tests/`
    - Configurar `@settings(max_examples=100)`
    - **Property 7: Detecção de testes é coerente com a estrutura do repositório**
    - **Validates: Requirements 1.6, 1.7**

- [~] 10. Checkpoint final — Executar suíte completa de testes
  - Garantir que todos os testes passam, ask the user if questions arise.

## Notes

- Tasks marcadas com `*` são opcionais e podem ser puladas para um MVP mais rápido
- A tarefa 3.2 (deletar `pipeline_aulas_10_11.yml`) é sequencialmente dependente da 3.1 e NÃO deve ser executada antes dela
- Os testes de propriedade (hypothesis) foram projetados para rodar sobre os arquivos YAML reais após criação; as tasks 8.2–8.4 dependem da existência de todos os workflows (task 7)
- Os helpers do `conftest.py` (task 8.1) são compartilhados por todos os testes — deve ser implementado antes dos testes de propriedade
- O workflow `ci_aula_06_09.yml` e `cd_projeto_final.yml` não requerem mudanças de código conforme o design
- Para executar os testes localmente: `pip install pytest hypothesis pyyaml && pytest tests/ -q`

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1.1", "2.1", "3.1", "4.1", "5.1", "6.1"] },
    { "id": 1, "tasks": ["1.2", "2.2", "3.2", "4.2", "5.2", "6.2"] },
    { "id": 2, "tasks": ["3.3", "8.1"] },
    { "id": 3, "tasks": ["8.2", "8.3", "8.4", "9.1", "9.2", "9.3"] }
  ]
}
```
