# Lab 13: Arquitetura Orientada a Filas (Queue), Processamento Assíncrono e Resiliência

## 🎯 Objetivos de Aprendizagem
- Aplicar a arquitetura Produtor-Consumidor usando filas (`queue.Queue`).
- Entender o conceito de IO Assíncrono com a biblioteca `asyncio`.
- Configurar rotação avançada de logs corporativos usando `RotatingFileHandler`.
- Aplicar padrões de re-tentativa (*Retry Pattern*) com a biblioteca `tenacity` para auto-recuperação de erros de rede.

## 💼 Desafio de Mercado
Processar milhares de faturas ou registros sequencialmente bloqueia o robô e desperdiça poder computacional. Caso haja instabilidade na internet, uma falha simples no meio da fila paralisa todo o pipeline sem rastreabilidade. A engenharia avançada de RPA resolve isso com arquitetura assíncrona, filas de tarefas independentes e logs rotativos de auditoria.

---

## 📝 Enunciado (Aluno)

1. Crie o arquivo `bot_faturamento_avancado.py`.
2. Configurar o `logging` para utilizar um `RotatingFileHandler` gravando no arquivo `app_rpa.log` com limite de 1MB por arquivo.
3. Implemente a arquitetura Produtor-Consumidor utilizando `queue.Queue`:
   - **Produtor**: Função que lê uma lista de 10 IDs de faturas fictícias e as adiciona na fila.
   - **Consumidor**: Função worker que retira itens da fila e processa as faturas.
4. Adicione um mecanismo de *Retry* usando `tenacity` na função de envio/processamento para simular até 3 tentativas caso ocorra um erro de rede.

---

