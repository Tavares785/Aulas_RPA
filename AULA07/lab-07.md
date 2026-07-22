# Lab 07: Projeto Guiado — Sistema de Lançamento de Notas

## 🎯 Objetivos de Aprendizagem
- Integrar lógica de dados em memória (listas de dicionários) com automação desktop.
- Construir formulários simulados de entrada e atalhos de navegação via teclado (`Tab`, `Enter`).
- Calcular e exibir estatísticas operacionais de tempo poupado versus esforço manual.

## 💼 Desafio de Mercado
Coordenadores pedagógicos gastam horas digitando manualmente notas de turmas em portais acadêmicos antigos que não oferecem suporte a upload de planilhas.

---

## 📝 Enunciado (Aluno)

Imagine que a lista de alunos abaixo precisa ser lançada em uma planilha ou formulário ativo na tela:
```python
alunos = [
    {"matricula": "202601", "nota": "8.5"},
    {"matricula": "202602", "nota": "4.0"},
    {"matricula": "202603", "nota": "9.8"}
]