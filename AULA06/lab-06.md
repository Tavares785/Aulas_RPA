# Lab 06: Controle de Periféricos e Segurança Fail-Safe

## 🎯 Objetivos de Aprendizagem
- Controlar teclado e mouse através da biblioteca `pyautogui`.
- Implementar medidas de segurança operacional (`PAUSE` e `FAILSAFE`).
- Mapear coordenadas do monitor ($X, Y$) e atalhos globais do sistema operacional.

## 💼 Desafio de Mercado
Muitos sistemas legados das empresas não possuem API nem suporte a ferramentas de automação avançadas. Nesses cenários, a interação por camadas de UI gráfica simula com precisão as ações humanas do operador.

---

## 📝 Enunciado (Aluno)

1. Instale a biblioteca `pyautogui`.
2. Crie o script `notepad_bot.py`.
3. Configure `pyautogui.FAILSAFE = True` e `pyautogui.PAUSE = 0.5`.
4. Desenvolva o robô que execute os seguintes passos no Windows/Linux:
   - Pressione a tecla `Win` (ou `Super`).
   - Digite `"notepad"` (ou `"gedit"` no Linux) e pressione `Enter`.
   - Aguarde 2 segundos para o aplicativo carregar.
   - Digite o seguinte texto de forma visível: `"Relatório de Execução Automática - RPA PyAutoGUI Ativo!"`.
   - Execute o atalho para salvar o arquivo (`Ctrl + S`), digite o nome do arquivo `"status_bot.txt"` e confirme com `Enter`.

---