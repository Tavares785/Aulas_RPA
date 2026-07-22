# Lab 11: Extração de Dados da Web em Escala com BeautifulSoup

## 🎯 Objetivos de Aprendizagem
- Fazer requisições HTTP seguras usando a biblioteca `requests`.
- Parsear a árvore DOM do HTML com a classe `BeautifulSoup`.
- Navegar e extrair dados via métodos `find()`, `find_all()` e seletores CSS (`select`).
- Aplicar princípios éticos de Web Scraping e tratamento do arquivo `robots.txt`.

## 💼 Desafio de Mercado
Diferente do Selenium (que simula um navegador real e consome muita memória/CPU), o `BeautifulSoup` combinado com `requests` realiza raspagem de dados extremamente veloz em sites estáticos ou server-side rendered, sendo a ferramenta perfeita para monitoramento de preços de concorrentes ou agregação de notícias.

---

## 📝 Enunciado (Aluno)

1. Instale as bibliotecas `requests` e `beautifulsoup4`.
2. Crie o script `scraper_noticias.py`.
3. O bot deve fazer uma requisição HTTP para a página de testes ou portal estático (ex: `https://quotes.toscrape.com/`).
4. Valide se a resposta do servidor foi `200 OK`.
5. Utilize o `BeautifulSoup` com o parser `'html.parser'` para converter o conteúdo.
6. Raspe todos os elementos de citações/frases e seus respectivos autores.
7. Limpe o texto extraído removendo espaços desnecessários (`.strip()`).
8. Exiba os 5 primeiros resultados no terminal e salve a lista completa em um arquivo CSV.

---

