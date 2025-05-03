
# 🕵️‍♂️ Desafio RPA - Portal da Transparência

Este projeto tem como objetivo automatizar a coleta de informações públicas do **Portal da Transparência**, especificamente dados de **pessoas físicas beneficiárias de programas sociais**.

A automação coleta:
- Nome completo
- CPF
- Localidade
- Lista de benefícios (detalhes)
- Captura de tela (base64) da página de perfil

---

## ⚙️ Tecnologias Utilizadas

- **Python 3.9+**
- **Selenium** para automação do navegador
- **Google Chrome** com `chromedriver`
- **JSON** para armazenar os dados extraídos
- **Base64** para captura e armazenamento de screenshots

---

## 📁 Estrutura dos Dados

Cada item salvo no `dados_transparencia.json` terá o seguinte formato:

```json
{
  "nome": "João da Silva",
  "cpf": "123.456.789-00",
  "localidade": "São Paulo/SP",
  "screenshot_base64": "<base64_string>",
  "beneficios": [
    ["Bolsa Família", "R$ 600,00", "Janeiro/2023"],
    ["Auxílio Brasil", "R$ 400,00", "Fevereiro/2023"]
  ]
}
```

---

## 🚀 Como Executar o Projeto

### 1. Instale as dependências

```bash
pip install selenium
```

> Certifique-se de que o `chromedriver` está instalado e compatível com sua versão do Google Chrome.

### 2. Execute o script

```bash
python desafio_rpa.py
```

O script abrirá o navegador, navegará pelo portal e salvará o arquivo `dados_transparencia.json` no mesmo diretório do script.

---

## 📌 Funcionalidades

- **Acesso automatizado** ao portal: preenche filtros e realiza consulta por pessoas físicas.
- **Extração de dados individuais**: nome, CPF, localidade.
- **Captura de tela em base64** de cada página individual.
- **Coleta de detalhes dos benefícios**.
- **Geração de JSON estruturado** com os dados.

---

## ⚠️ Considerações

- O código trabalha com delays (`time.sleep`) para evitar falhas de carregamento.
- Em caso de erro em uma pessoa, o código continuará com as demais.
- Se o site estiver fora do ar ou muito lento, a execução pode falhar.
- O uso em massa deste tipo de automação deve seguir os termos de uso do site e princípios éticos de RPA.
- O LINK Para o registro em video do robô em atividade (exemplo com uma ou mais consultas): https://youtu.be/KnvLbvMqTPM ⚠️⚠️⚠️⚠️⚠️
---

## 🧪 Sugestão de Melhorias Futuras

- Implementar logs detalhados com timestamps.
- Adicionar suporte a paginação para coletar mais de uma página.
- Exportar também em formato CSV ou Excel.
- Integrar com banco de dados relacional.

---

## 👨‍💻 Autor

**Igor Augusto de Gois**  
📍 São Paulo - SP  
🛠 Experiência em automações desde 2021  
