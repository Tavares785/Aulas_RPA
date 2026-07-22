# Lab 08: Web Scraping e Interação com Elementos via Selenium

## 🎯 Objetivos de Aprendizagem
- Configurar e instanciar o Selenium WebDriver (`webdriver.Chrome`)
- Mapear elementos HTML usando seletores estáticos (`By.ID`, `By.NAME`)
- Implementar esperas implícitas (`implicitly_wait`) para mitigar assincronismo do navegador

## 💼 Desafio de Mercado
Ao contrário da automação Desktop (baseada em imagem ou coordenadas estáticas), a automação Web inspeciona a árvore DOM do navegador Isso garante alta performance e independe de resolução de tela

---

## 📝 Enunciado (Aluno)

1. Instale a biblioteca `selenium` e `webdriver-manager`
2. Crie o script `bot_web_login.py`
3. O bot deve abrir o navegador Chrome no site de treinos: `https://the-internet.herokuapp.com/login`
4. Configure um `implicitly_wait(10)`
5. Localize os elementos:
   - Campo de Usuário (por ID `username`) -> Preencher com `"tomsmith"`
   - Campo de Senha (por ID `password` ou NAME `password`) -> Preencher com `"SuperSecretPassword!"`
   - Botão de Login (por CSS Selector ou Tag) -> Clicar no botão
6. Valide se o login foi bem-sucedido extraindo e exibindo a mensagem exibida no elemento de id `flash`

---