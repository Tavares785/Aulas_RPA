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

