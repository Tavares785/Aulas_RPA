# Lab 03: Arquitetura Modular de Dados de Processamento

## 🎯 Objetivos de Aprendizagem
- Modelar entidades de negócio utilizando Dicionários e Listas.
- Modularizar o código do robô através de Funções com parâmetros e retornos explicitados.
- Construir um menu interativo e manipulador de dados dinâmico.

## 💼 Desafio de Mercado
Um sistema de RH necessita de uma automação que cadastre colaboradores em memória antes de enviá-los ao sistema legado. É necessário um módulo isolado que valide as entradas e estruture os dados do colaborador em dicionários padronizados.

---

## 📝 Enunciado (Aluno)

1. Crie um arquivo chamado `mod_rh.py` e nele desenvolva as seguintes funções:
   - `cadastrar_colaborador(nome: str, cargo: str, salario: float) -> dict`: Retorna um dicionário estruturado com as chaves `"nome"`, `"cargo"`, `"salario"`.
   - `exibir_colaboradores(lista_colaboradores: list) -> None`: Percorre a lista e imprime os colaboradores formatados.
2. Crie um script principal `main.py` contendo um loop `while True` para gerenciar um menu interativo com as opções: `1 - Cadastrar`, `2 - Listar`, `0 - Sair`.

---