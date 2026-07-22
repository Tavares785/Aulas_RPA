# Lab 12: Integração de Sistemas via APIs REST (Requests) e Notificações E-mail (SMTP)

## 🎯 Objetivos de Aprendizagem
- Entender a diferença entre automação por UI e comunicação nativa por protocolo de rede[cite: 12].
- Consumir Web APIs REST utilizando verbos HTTP (`GET`) e manipular JSON em dicionários Python[cite: 12].
- Construir e disparar e-mails automáticos via protocolo SMTP usando as bibliotecas `smtplib` e `email.mime`[cite: 12].

## 💼 Desafio de Mercado
Acionar navegadores por interface gráfica para buscar informações simples (ex: cotação de moedas, validação de CEP ou envio de alertas) gera lentidão e fragilidade[cite: 12]. A forma profissional e escalável de automação conecta-se diretamente às APIs dos serviços e envia notificações por e-mail[cite: 12].

---

## 📝 Enunciado (Aluno)

Crie um script integrador `bot_cotacao_alerta.py` que execute os passos[cite: 12]:
1. **Consumo de API:** Acesse a API pública da AwesomeAPI para buscar a cotação atual do Dólar (`https://economia.awesomeapi.com.br/json/last/USD-BRL`)[cite: 12].
2. **Processamento JSON:** Extraia o valor de compra (`bid`) do Dólar do JSON retornado[cite: 12].
3. **Simulação de Notificação por E-mail (SMTP):**
   - Construa uma função que receba a cotação extraída e monte uma mensagem de e-mail formatada com a biblioteca `email.mime`[cite: 12].
   - Configure a estrutura do protocolo SMTP usando `smtplib.SMTP`[cite: 12].
   *(Nota: Para evitar dependência de credenciais reais do aluno durante o lab, permita um modo mock de impressão do e-mail no terminal)*[cite: 12].

---

## 🔑 Gabarito e Resolução Comentada (Professor)

```python
"""
Script: bot_cotacao_alerta.py
Descrição: Integração de API REST pública de cotação com envio de e-mail SMTP.
Autor: ProfBot - ADS
"""

import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import requests

def buscar_cotacao_dolar() -> float:
    """Consome a API REST pública de moedas e retorna a cotação do Dólar."""
    url_api = "[https://economia.awesomeapi.com.br/json/last/USD-BRL](https://economia.awesomeapi.com.br/json/last/USD-BRL)"
    print(" Consultando API de cotação do Dólar...")

    resposta = requests.get(url_api, timeout=10)
    
    if resposta.status_code == 200:
        dados_json = resposta.json()
        cotacao_compra = float(dados_json["USDBRL"]["bid"])
        return cotacao_compra
    else:
        raise Exception(f"Erro ao consultar API: Status Code {resposta.status_code}")

def enviar_email_alerta(cotacao: float, destino_email: str, modo_simulacao: bool = True):
    """Constrói e envia mensagem via protocolo SMTP."""
    remetente_email = "bot.rpa.notificacao@gmail.com"
    
    # Construção da Mensagem MIME
    mensagem = MIMEMultipart()
    mensagem["From"] = remetente_email
    mensagem["To"] = destino_email
    mensagem["Subject"] = f" 📊 ALERTA RPA: Cotação do Dólar Hoje (R$ {cotacao:.2f})"

    corpo_texto = (
        f"Olá,\n\n"
        f"O robô de monitoramento informa a cotação atual do Dólar:\n"
        f" Valor de Compra: R$ {cotacao:.2f}\n\n"
        f"Este é um e-mail gerado automaticamente por um robô de RPA em Python."
    )
    mensagem.attach(MIMEText(corpo_texto, "plain"))

    if modo_simulacao:
        print("\n [MODO SIMULAÇÃO/MOCK ATIVO - E-mail não enviado ao servidor real]")
        print("="*60)
        print(f"De: {mensagem['From']}\nPara: {mensagem['To']}\nAssunto: {mensagem['Subject']}\n")
        print(corpo_texto)
        print("="*60)
        return

    # Conexão Real com Servidor SMTP (Gmail Exemplo)
    try:
        print(" Conectando ao servidor SMTP...")
        servidor = smtplib.SMTP("smtp.gmail.com", 587)
        servidor.starttls() # Criptografia de canal
        servidor.login(remetente_email, "sua_senha_de_aplicativo_aqui")
        servidor.sendmail(remetente_email, destino_email, mensagem.as_string())
        servidor.quit()
        print(" E-mail disparado com sucesso via SMTP!")
    except Exception as e:
        print(f" [ERRO SMTP] Falha no envio do e-mail: {e}")

if __name__ == "__main__":
    try:
        valor_dolar = buscar_cotacao_dolar()
        print(f" Cotação capturada: R$ {valor_dolar:.2f}")
        
        # Chamada em modo simulação pedagógica
        enviar_email_alerta(valor_dolar, destino_email="gestor@empresa.com", modo_simulacao=True)
        
    except Exception as err:
        print(f" [FALHA NO PIPELINE]: {err}")