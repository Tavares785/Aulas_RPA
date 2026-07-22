# Lab 09: Manipulação de Dropdowns, Pop-ups e Web Elements Complexos

## 🎯 Objetivos de Aprendizagem
- Mapear elementos dinâmicos usando `XPath` e `CSS Selectors` avançados[cite: 9].
- Lidar com caixas de seleção suspensas através da classe `Select`[cite: 9].
- Trocar contexto de execução para operar dentro de `Iframes`[cite: 9].
- Capturar, aceitar ou rejeitar caixas de diálogo nativas do navegador (`Alerts`)[cite: 9].

## 💼 Desafio de Mercado
A maioria das aplicações corporativas web possui seletores dinâmicos, janelas modais de confirmação, estruturas aninhadas em `iframes` e formulários com dropdowns[cite: 9]. Dominar essas interações é crucial para criar automações web resilientes em sistemas legados.

---

## 📝 Enunciado (Aluno)

1. Crie o script `web_avancado.py`[cite: 9].
2. **Parte 1 (Selects):** Acesse a página `https://the-internet.herokuapp.com/dropdown`[cite: 9]. Utilize a classe `Select` para selecionar a opção **"Option 2"** pelo texto visível[cite: 9].
3. **Parte 2 (Alerts JS):** Acesse `https://the-internet.herokuapp.com/javascript_alerts`[cite: 9]. Clique no botão que dispara o alerta JS via XPath (`"//button[contains(text(), 'Click for JS Alert')]"`), capture o texto contido no alerta, exiba no terminal e confirme (`accept()`) o alerta nativo[cite: 9].
4. **Parte 3 (Iframes):** Acesse `https://the-internet.herokuapp.com/iframe`[cite: 9]. Mude o foco para o Iframe de ID `"mce_0_ifr"`, limpe o editor de texto, escreva uma mensagem personalizada e retorne ao documento principal (`default_content()`)[cite: 9].