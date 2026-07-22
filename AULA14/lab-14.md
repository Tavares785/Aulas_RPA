# Lab 14: Arquitetura de Software RPA, Gitignore e Estrutura de Repositório

## 🎯 Objetivos de Aprendizagem
- Planejar a arquitetura modular de uma solução completa de RPA[cite: 14].
- Redigir a documentação técnica oficial (`README.md`) para o ecossistema GitHub[cite: 14].
- Configurar arquivos de proteção e boas práticas (`.gitignore`) para evitar vazamento de credenciais[cite: 14].
- Organizar a estrutura de diretórios e dependências (`requirements.txt`)[cite: 14].

## 💼 Desafio de Mercado
Um projeto de RPA sem arquitetura definida vira um "script monolítico gigante" impossível de manter[cite: 14]. Em ambientes corporativos de engenharia de software, o repositório deve ser limpo, modular, ter dependências declaradas e proteger dados sensíveis de clientes[cite: 14].

---

## 📝 Enunciado (Aluno)

1. Crie a pasta do repositório final do seu grupo: `rpa-projeto-final`[cite: 14].
2. Crie a estrutura de diretórios do projeto[cite: 14]:
   - `config/` (para arquivos de configuração)
   - `modules/` (para módulos reutilizáveis)
   - `data/` (para entrada/saída de planilhas e PDFs)
3. Crie o arquivo `.gitignore` configurado para ignorar ambiente virtual (`.venv/`), logs (`*.log`), e dados das pastas de saída (`data/*.xlsx`)[cite: 14].
4. Crie o arquivo `requirements.txt` listando as bibliotecas utilizadas (ex: `pandas`, `selenium`, `requests`)[cite: 14].
5. Redija o arquivo `README.md` em Markdown detalhando a arquitetura e a especificação da solução[cite: 14].

---

## 🔑 Gabarito e Resolução Comentada (Professor)

### 1. Estrutura de Arquivos Esperada no Repositório

```text
rpa-projeto-final/
│
├── .gitignore
├── requirements.txt
├── README.md
├── main.py
│
├── config/
│   └── settings.py
│
├── modules/
│   ├── __init__.py
│   ├── web_scraper.py
│   └── pdf_processor.py
│
└── data/
    ├── input/
    └── output/