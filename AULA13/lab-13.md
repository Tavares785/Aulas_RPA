# Lab 13: Arquitetura Orientada a Filas (Queue), Processamento Assíncrono e Resiliência

## 🎯 Objetivos de Aprendizagem
- Aplicar a arquitetura Produtor-Consumidor usando filas (`queue.Queue`)[cite: 13].
- Entender o conceito de IO Assíncrono com a biblioteca `asyncio`[cite: 13].
- Configurar rotação avançada de logs corporativos usando `RotatingFileHandler`[cite: 13].
- Aplicar padrões de re-tentativa (*Retry Pattern*) com a biblioteca `tenacity` para auto-recuperação de erros de rede[cite: 13].

## 💼 Desafio de Mercado
Processar milhares de faturas ou registros sequencialmente bloqueia o robô e desperdiça poder computacional[cite: 13]. Caso haja instabilidade na internet, uma falha simples no meio da fila paralisa todo o pipeline sem rastreabilidade[cite: 13]. A engenharia avançada de RPA resolve isso com arquitetura assíncrona, filas de tarefas independentes e logs rotativos de auditoria[cite: 13].

---

## 📝 Enunciado (Aluno)

1. Crie o arquivo `bot_faturamento_avancado.py`[cite: 13].
2. Configurar o `logging` para utilizar um `RotatingFileHandler` gravando no arquivo `app_rpa.log` com limite de 1MB por arquivo[cite: 13].
3. Implemente a arquitetura Produtor-Consumidor utilizando `queue.Queue`:
   - **Produtor**: Função que lê uma lista de 10 IDs de faturas fictícias e as adiciona na fila[cite: 13].
   - **Consumidor**: Função worker que retira itens da fila e processa as faturas[cite: 13].
4. Adicione um mecanismo de *Retry* usando `tenacity` na função de envio/processamento para simular até 3 tentativas caso ocorra um erro de rede[cite: 13].

---

## 🔑 Gabarito e Resolução Comentada (Professor)

```python
"""
Script: bot_faturamento_avancado.py
Descrição: Padrão Produtor-Consumidor com Filas, Retry e Logging Rotativo.
Autor: ProfBot - ADS
"""

import queue
import time
import random
import logging
from logging.handlers import RotatingFileHandler
from tenacity import retry, stop_after_attempt, wait_fixed

# 1. Configuração de Logging Corporativo Rotativo
handler_rotativo = RotatingFileHandler(
    "app_rpa.log", 
    maxBytes=1024*1024, # 1 MB por arquivo
    backupCount=3,       # Mantém até 3 arquivos históricos
    encoding="utf-8"
)
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] (Worker) %(message)s",
    handlers=[handler_rotativo, logging.StreamHandler()]
)

# 2. Instância da Fila Global de Trabalhos
fila_processamento = queue.Queue()

# 3. Função com Padrão Retry (Tenta até 3 vezes com intervalo de 1s)
@retry(stop=stop_after_attempt(3), wait=wait_fixed(1))
def enviar_fatura_api(id_fatura: int):
    """Simula o envio de uma fatura para uma API instável."""
    logging.info(f"Tentando enviar Fatura #{id_fatura} para a API...")
    
    # Simulação de erro aleatório em 30% das chamadas
    if random.random() < 0.3:
        logging.warning(f" Erro de conexão temporário ao enviar Fatura #{id_fatura}. Disparando Retry...")
        raise ConnectionError("Timeout de conexão com o servidor de faturamento!")

    logging.info(f" Fatura #{id_fatura} processada e confirmada com sucesso!")

# 4. Consumidor (Worker)
def trabalhador_consumidor():
    while not fila_processamento.empty():
        id_fatura = fila_processamento.get()
        try:
            enviar_fatura_api(id_fatura)
        except Exception as e:
            logging.error(f" [FALHA DEFINITIVA] Fatura #{id_fatura} falhou após todas as tentativas: {e}")
        finally:
            fila_processamento.task_done()

# 5. Produtor
def produtor_carregar_fila(total_itens: int = 10):
    logging.info("--- INICIANDO CARGA DA FILA DE TRABALHO (PRODUTOR) ---")
    for i in range(101, 101 + total_itens):
        fila_processamento.put(i)
        logging.info(f"Item adicionado à fila: Fatura #{i}")

def main():
    produtor_carregar_fila(total_itens=8)
    
    logging.info("\n--- INICIANDO PROCESSAMENTO DOS CONSUMIDORES ---")
    trabalhador_consumidor()

    fila_processamento.join() # Aguarda o esvaziamento completo da fila
    logging.info("--- PIPELINE DE FATURAMENTO FINALIZADO ---")

if __name__ == "__main__":
    main()