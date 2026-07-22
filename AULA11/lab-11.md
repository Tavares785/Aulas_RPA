# Lab 11: Extração de Dados da Web em Escala com BeautifulSoup

## 🎯 Objetivos de Aprendizagem
- Fazer requisições HTTP seguras usando a biblioteca `requests`[cite: 11].
- Parsear a árvore DOM do HTML com a classe `BeautifulSoup`[cite: 11].
- Navegar e extrair dados via métodos `find()`, `find_all()` e seletores CSS (`select`)[cite: 11].
- Aplicar princípios éticos de Web Scraping e tratamento do arquivo `robots.txt`[cite: 11].

## 💼 Desafio de Mercado
Diferente do Selenium (que simula um navegador real e consome muita memória/CPU)[cite: 11], o `BeautifulSoup` combinado com `requests` realiza raspagem de dados extremamente veloz em sites estáticos ou server-side rendered[cite: 11], sendo a ferramenta perfeita para monitoramento de preços de concorrentes ou agregação de notícias[cite: 11].

---

## 📝 Enunciado (Aluno)

1. Instale as bibliotecas `requests` e `beautifulsoup4`.
2. Crie o script `scraper_noticias.py`[cite: 11].
3. O bot deve fazer uma requisição HTTP para a página de testes ou portal estático (ex: `https://quotes.toscrape.com/`)[cite: 11].
4. Valide se a resposta do servidor foi `200 OK`.
5. Utilize o `BeautifulSoup` com o parser `'html.parser'` para converter o conteúdo[cite: 11].
6. Raspe todos os elementos de citações/frases e seus respectivos autores[cite: 11].
7. Limpe o texto extraído removendo espaços desnecessários (`.strip()`)[cite: 11].
8. Exiba os 5 primeiros resultados no terminal e salve a lista completa em um arquivo CSV[cite: 11].

---

## 🔑 Gabarito e Resolução Comentada (Professor)

```python
"""
Script: scraper_noticias.py
Descrição: Web Scraping de alta performance com Requests e BeautifulSoup.
Autor: ProfBot - ADS
"""

import csv
import time
import requests
from bs4 import BeautifulSoup

def executar_scraper():
    url_alvo = "[https://quotes.toscrape.com/](https://quotes.toscrape.com/)"
    
    # Headers para simular uma requisição legítima de navegador (Boas Práticas)
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"
    }

    print(f" Conectando ao servidor: {url_alvo}...")
    
    try:
        resposta = requests.get(url_alvo, headers=headers, timeout=10)
        
        # Validação do código de status HTTP
        if resposta.status_code != 200:
            print(f" [ERRO] O servidor respondeu com status código: {resposta.status_code}")
            return

        print(" Pagina baixada com sucesso. Iniciando parseamento HTML...")
        soup = BeautifulSoup(resposta.text, 'html.parser')

        # Localizando os blocos de citação usando seletores CSS
        blocos_citacoes = soup.select("div.quote")
        
        dados_extraidos = []

        for bloco in blocos_citacoes:
            # Extração de dados internos
            texto_citacao = bloco.find("span", class_="text").get_text(strip=True)
            autor = bloco.find("small", class_="author").get_text(strip=True)

            dados_extraidos.append({
                "autor": autor,
                "citacao": texto_citacao
            })

        print(f" Extraídos {len(dados_extraidos)} registros da página.")

        # Salvando os dados extraídos em CSV
        caminho_csv = "citacoes_extraidas.csv"
        with open(caminho_csv, mode="w", encoding="utf-8", newline="") as arquivo_csv:
            colunas = ["autor", "citacao"]
            writer = csv.DictWriter(arquivo_csv, fieldnames=colunas)
            writer.writeheader()
            writer.writerows(dados_extraidos)

        print(f" Arquivo '{caminho_csv}' gerado com sucesso!")

    except requests.exceptions.RequestException as req_err:
        print(f" [FALHA DE REDE] Erro ao tentar acessar o site: {req_err}")

if __name__ == "__main__":
    executar_scraper()