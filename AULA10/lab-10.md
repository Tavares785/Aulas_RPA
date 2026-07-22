# Lab 10: Extração de Dados em PDFs, Filtros Regex e Exportação para Excel

## 🎯 Objetivos de Aprendizagem
- Extrair texto não estruturado de arquivos PDF usando `pdfplumber`.
- Criar e aplicar Expressões Regulares (`re`) para localizar padrões específicos (NIF, Valores, Datas) no texto bruto.
- Estruturar dados extraídos em DataFrames e exportá-los para Excel com a biblioteca `pandas`.

## 💼 Desafio de Mercado
Em setores contábeis e de suprimentos, milhares de faturas em PDF chegam diariamente por e-mail sem padronização do leiaute visual. O operador humano gasta horas abrindo cada PDF, copiando o NIF do fornecedor e o valor total para uma planilha. O robô deve automatizar esse pipeline extraindo os padrões e persistindo a tabela em Excel.

---

## 📝 Enunciado (Aluno)

1. Certifique-se de ter instalado as bibliotecas `pdfplumber`, `pandas` e `openpyxl`.
2. Crie o script `leitor_faturas_pdf.py`.
3. O script deve simular a leitura de faturas em PDF extraindo o texto de cada página.
4. Escreva regras de Expressões Regulares (`re.search`) para isolar:
   - **NIF/CNPJ**: Padrão de 9 dígitos numéricos ou formato `XX.XXX.XXX/XXXX-XX`.
   - **Valor Total**: Padrão numérico monetário (ex: `R$ 1.500,00` ou `Total: 1500.00`).
5. Armazene as informações de cada fatura em uma lista de dicionários.
6. Converta a lista em um DataFrame do `pandas` e salve em um arquivo Excel `relatorio_faturas.xlsx`.

---

