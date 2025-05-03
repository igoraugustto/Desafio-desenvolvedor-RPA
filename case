# Importações necessárias
import time  # Utilizado para pausas no código (delays)
import json  # Para salvar dados em JSON
import base64  # Caso precise converter ou manipular o base64
from selenium import webdriver  # Webdriver para controle do navegador
from selenium.webdriver.common.by import By  # Para localizar elementos
from selenium.webdriver.chrome.options import Options  # Para configurar o Chrome
from selenium.webdriver.support.ui import WebDriverWait  # Espera explícita
from selenium.webdriver.support import expected_conditions as EC  # Condições de espera

# URL principal do Portal da Transparência (visão geral de pessoa física)
PORTAL_URL = "https://portaldatransparencia.gov.br/pessoa/visao-geral"

# Configurações do navegador Chrome
options = Options()
options.add_argument("--disable-notifications")  # Desativa notificações do navegador

# Inicia o navegador com as opções definidas
driver = webdriver.Chrome(options=options)
driver.get(PORTAL_URL)  # Acessa o link
driver.maximize_window()  # Maximiza a janela
driver.implicitly_wait(15)  # Espera implícita para carregamento de elementos



# Clica no botão "Consulta Pessoa Física"
driver.execute_script("document.getElementById('button-consulta-pessoa-fisica').click()")
time.sleep(0.5)

# Aceita os cookies
driver.execute_script("document.getElementById('accept-minimal-btn').click()")

# Expande a seção do formulário
driver.execute_script("document.getElementsByClassName('header')[2].click()")
time.sleep(0.5)

# Marca a opção "Beneficiário de Programa Social"
driver.execute_script("document.getElementById('beneficiarioProgramaSocial').click()")
time.sleep(0.5)

# Clica em "Consultar"
driver.execute_script("document.getElementById('btnConsultarPF').click()")
time.sleep(5)  # Aguarda carregamento da página de resultados

# Coleta todos os nomes clicáveis da primeira página de resultados
pessoas = driver.find_elements(By.CLASS_NAME, 'link-busca-nome')

# Lista para armazenar os dados coletados
dados_gerais = []

# Loop por cada pessoa da lista
for i, pessoa in enumerate(pessoas):
    try:
        # Clica no nome da pessoa (acesso à página individual)
        driver.find_elements(By.CLASS_NAME, 'link-busca-nome')[i].click()
        time.sleep(2)

        print(f"Processando pessoa número {i}")
        time.sleep(1)

        # Expande a seção com os dados
        driver.find_elements(By.CLASS_NAME, "header")[2].click()
        time.sleep(1)

        # Coleta nome, CPF e localidade
        nome_completo = driver.find_element(By.XPATH, "//div[strong[text()='Nome']]/span").text.strip()
        cpf = driver.find_element(By.XPATH, "//div[strong[text()='CPF']]/span").text.strip()
        localidade = driver.find_element(By.XPATH, "//div[strong[text()='Localidade']]/span").text.strip()

        print(f"[{i}] Nome: {nome_completo} | CPF: {cpf} | Localidade: {localidade}")

        # Tira screenshot da tela atual e armazena como base64
        imagem_base64 = driver.get_screenshot_as_base64()

        # Lista que armazenará os benefícios dessa pessoa
        beneficios_pessoa = []

        # Encontra os botões de "Detalhes"
        botoes = driver.find_elements(By.CLASS_NAME, "secondary")

        # Percorre os botões a partir do segundo (índice 1)
        for j in range(1, len(botoes)):
            try:
                # Recarrega a página da pessoa
                driver.refresh()
                time.sleep(1)
                driver.execute_script("document.body.style.zoom = '0.5'")

                # Reabre a seção dos detalhes da pessoa
                driver.find_elements(By.CLASS_NAME, "header")[2].click()
                time.sleep(1)

                # Atualiza os botões após o refresh
                botoes_atualizados = driver.find_elements(By.CLASS_NAME, "secondary")
                botoes_atualizados[j].click()  # Clica no botão do benefício

                # Aguarda a tabela de benefícios aparecer
                WebDriverWait(driver, 10).until(
                    EC.presence_of_element_located((By.XPATH, '//tbody/tr'))
                )

                # Coleta as linhas da tabela
                linhas = driver.find_elements(By.XPATH, '//tbody/tr')
                for linha in linhas:
                    colunas = linha.find_elements(By.TAG_NAME, 'td')
                    valores = [coluna.text.strip() for coluna in colunas]
                    beneficios_pessoa.append(valores)

                time.sleep(2)
                driver.back()  # Volta para a página anterior (dados da pessoa)
            except Exception as e:
                driver.back()
                time.sleep(5)
                break  # Sai do loop de botões

        # Adiciona os dados coletados desta pessoa no JSON
        dados_gerais.append({
            "nome": nome_completo,
            "cpf": cpf,
            "localidade": localidade,
            "screenshot_base64": imagem_base64,  # Adiciona imagem base64
            "beneficios": beneficios_pessoa
        })

    except Exception as e:
        continue  # Se der erro com a pessoa atual, continua com a próxima

# Salva todos os dados no arquivo JSON
with open("dados_transparencia.json", "w", encoding="utf-8") as f:
    json.dump(dados_gerais, f, ensure_ascii=False, indent=4)

print("Coleta finalizada. Dados salvos em 'dados_transparencia.json'.")
