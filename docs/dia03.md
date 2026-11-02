# Agentes + Visão Computacional

---

### 🛠️ Ferramentas Utilizadas

---

<div style="inline-flex">
<a href="https://colab.new" target="_blank"><img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Google Colab"></a>
<img src="https://img.shields.io/badge/Ultralytics%20YOLO-111F68?style=for-the-badge&logo=yolo&logoColor=white" alt="Ultralytics YOLO">
<img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" alt="OpenCV">
<img src="https://img.shields.io/badge/MCP-000000?style=for-the-badge&logo=anthropic&logoColor=white" alt="Model Context Protocol">
<img src="https://img.shields.io/badge/Telegram%20Bot%20API-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Bot API">
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
</div>

---

### 🤖 1. O que é um Agente de IA?

---

Nos **Dias 01 e 02**, aprendemos a manipular pixels e a utilizar modelos como o **YOLO** para detectar objetos em imagens e vídeos. Porém, um detector convencional apenas retorna dados brutos (caixas, classes e probabilidades). Como transformar essas detecções em **tomada de decisão e ação no mundo real**?

É aqui que entra o conceito de **Agente de IA**: um sistema que **observa** um ambiente, **processa** informações e **escolhe ações** para alcançar um objetivo específico.

<img src="../assets/images/ciclo_agente_visao.gif" alt="Animação mostrando o ciclo de funcionamento de um Agente de Visão: 1. Observação, 2. Interpretação, 3. Decisão, 4. Ação e 5. Resultado com feedback contínuo">

* **Combinação de técnicas:** Um agente pode combinar modelos de aprendizado de máquina (como redes neurais), regras de negócio e funções Python. **Nem todo agente precisa usar um Modelo de Linguagem (LLM)** — agentes baseados em regras ou políticas determinísticas são rápidos, previsíveis e amplamente usados na indústria.
* **Agente de Visão (*Vision Agent*):** Utiliza imagens ou vídeos como **observações** do ambiente. Enquanto um detector comum apenas lista os objetos identificados, o agente de visão usa essas detecções para escolher verificações adicionais, acionar ferramentas externas e decidir o próximo passo.

---

### ⚖️ 2. Bot vs. Agente e Níveis de Autonomia

---

#### Bot e Agente: qual é a diferença?

Um **bot** automatiza interações ou tarefas repetitivas seguindo um roteiro fixo (*gatilho → regra → resposta*). Já um **agente** persegue um objetivo: ele planeja passos, usa ferramentas, observa o resultado e ajusta a rota quando necessário.

> **Importante:** Os conceitos não são excludentes! Um **bot** (como um bot do Telegram) pode fazer parte de um sistema **agente**, atuando como o canal de comunicação ou ferramenta de notificação do agente.

<img src="../assets/images/bot_vs_agente_recife.png" alt="Comparativo visual entre Bot e Agente de IA">

#### Níveis de Autonomia de Agentes de IA

Dependendo de quanto controle o humano mantém sobre o sistema e de como o agente planeja suas ações, classificamos os agentes em **5 níveis de autonomia** — divididos entre aqueles em que o **usuário é ativo no fluxo (L1 a L3)** e aqueles em que o **usuário é passivo (L4 e L5)**, apenas aprovando bloqueios ou monitorando o sistema:

<img src="../assets/images/niveis_autonomia_agentes_ia.png" alt="Níveis de autonomia de agentes de IA">

<p style="text-align: center;"> Artigo original: <a href="https://arxiv.org/html/2506.12469v1" target="_blank">Levels of Autonomy for AI Agents Working Paper.</a></p>

> **No nosso projeto prático:** Construiremos um agente que opera com **política determinística em código** (onde definimos os critérios de decisão e o agente executa o monitoramento de forma autônoma) e veremos como ele pode ser expandido com um **LLM** para planejar passos de forma mais flexível.

---

### 🔌 3. Model Context Protocol (MCP)

---

Quando construímos agentes que precisam acionar diversas funções (consultar bancos de dados, verificar sensores, analisar imagens ou enviar mensagens), integrar cada ferramenta manualmente pode se tornar confuso.

O **Model Context Protocol (MCP)** é um protocolo aberto que padroniza a forma como aplicações de IA descobrem e utilizam ferramentas (*tools*) e recursos disponibilizados por servidores compatíveis.

<img src="../assets/images/arquitetura_mcp_visao.png" alt="Diagrama da Arquitetura Model Context Protocol (MCP) no Agente de Visão conectando o Host (Google Colab) ao Servidor MCP (FastMCP) com as ferramentas detectar_objetos, contar_objetos, verificar_condicao e enviar_alerta">

#### Componentes da Arquitetura MCP:

=== "Host (Anfitrião)"

    * **Papel:** É a aplicação principal que integra o modelo ou a política do agente e coordena toda a interação (por exemplo, o nosso notebook no Google Colab ou uma aplicação em produção).

=== "Cliente MCP"

    * **Papel:** Componente dentro do Host que mantém a conexão com o servidor MCP, consulta quais ferramentas estão disponíveis e solicita a execução de operações.

=== "Servidor MCP"

    * **Papel:** Expõe ferramentas padronizadas (como funções Python decoradas com `@mcp.tool()`) para que o cliente possa chamá-las de forma segura e estruturada.

> **Atenção:** O MCP **não é um modelo de IA** e não escolhe sozinho quais ferramentas usar. A decisão sempre cabe ao **agente** (seja via regras em código ou via LLM). No Google Colab, cliente e servidor podem ser testados na mesma sessão utilizando o `FastMCP`.

---

### 💻 4. Prática: Vision Agent para Detectar Mudanças entre Imagens

---

Nesta prática final, vamos integrar tudo o que construímos no minicurso para criar um **Vision Agent completo**, capaz de inspecionar imagens com **YOLO + OpenCV**, tomar decisões com base nas detecções, acionar ferramentas via **MCP** e enviar alertas automáticos no **Telegram**!

<img src="../assets/images/fluxo_pratico_vision_agent.png" alt="Fluxo prático do projeto Vision Agent comparando a cena antes e depois com YOLO, acionando verificar_condicao() e disparando alerta no Telegram">

#### Como o agente escolhe o que investigar?

=== "Versão Básica (Regras Determinísticas)"

    A versão padrão do nosso projeto utiliza **regras determinísticas em Python**:

    1. O agente observa a cena comparando duas imagens (*antes* e *depois*) com `detectar_objetos()` e `contar_objetos()`.
    2. Se houver presença ou variação na contagem da classe monitorada (ex.: `person`), a política do agente aciona `verificar_condicao(observacao)`.
    3. Caso a verificação confirme uma mudança relevante, a regra de ação dispara `enviar_alerta()` para notificar o usuário no Telegram (com texto e/ou imagem anotada).

=== "Versão com LLM (Planejador Autônomo)"

    Um **Modelo de Linguagem (LLM)** pode substituir ou complementar esse planejador determinístico:

    * Em vez de `if/else` fixos, o LLM lê a lista de ferramentas do Servidor MCP (a partir das *docstrings* e tipos do Python) e escolhe quais chamar com base no objetivo em linguagem natural.
    * Como essa opção exige acesso a um provedor de modelo e uma chave de API (`API Key`), ela é opcional e não é necessária para executar a versão básica do minicurso.

---

#### Passo 0 (Opcional): Como configurar seu Bot no Telegram

Nosso código funciona perfeitamente em **modo de simulação** (imprimindo o alerta na tela do Colab) caso você não queira configurar o Telegram agora. Mas, se quiser receber as notificações reais no seu celular, basta seguir estes 3 passos rápidos:

1. **Criar o Bot e pegar o `TOKEN`:** No Telegram, procure por **`@BotFather`**, envie o comando `/newbot`, escolha um nome e um `@username` terminado em `bot`. O BotFather responderá com o seu **Token de API** (ex.: `123456789:ABCdefGhIJKlmNoPQRsTUVwxyZ`).
2. **Iniciar conversa com o Bot:** Clique no link do seu novo bot e aperte **Começar (`Start`)** (ou envie um `"Olá"`). Um bot não pode enviar mensagens para quem nunca falou com ele antes!
3. **Descobrir o seu `CHAT_ID`:** Envie uma mensagem para o bot **`@userinfobot`** no Telegram (ele responde na hora com o seu `Id` numérico) ou acesse no navegador `https://api.telegram.org/bot<SEU_TOKEN>/getUpdates` após mandar um "Oi" para o seu bot e copie o número em `"chat": {"id": ...}`.

---

#### Passo 1: Instalação das Dependências

No Google Colab, instalamos o `ultralytics`, o SDK do `mcp` e a biblioteca `requests` para comunicação com a API do Telegram.

```python
# No Google Colab, descomente a linha abaixo para instalar:
# !pip install -q ultralytics opencv-python matplotlib requests mcp

import cv2
import requests
from ultralytics import YOLO

# Carrega o modelo YOLO pré-treinado
modelo = YOLO('yolov8n.pt')
```

---

#### Passo 2: Definindo as Ferramentas de Visão, Verificação e Alerta

Reutilizamos a base de detecção do **Dia 02** e criamos as funções de verificação de mudanças entre duas observações (imagem de referência vs. imagem atual) e de envio de alerta no Telegram (com suporte opcional ao envio da foto anotada pelo YOLO!):

```python
def detectar_objetos(caminho_ou_img, limite_confianca=0.5, salvar_anotada=''):
    """Executa o YOLO e retorna lista de dicionários com classe, confiança e coordenadas."""
    resultados = modelo.predict(source=caminho_ou_img, conf=limite_confianca, verbose=False)
    resultado = resultados[0]

    # Se um caminho for informado, salva a imagem com as caixas desenhadas pelo YOLO
    if salvar_anotada:
        cv2.imwrite(salvar_anotada, resultado.plot())

    deteccoes = []
    for caixa in resultado.boxes:
        id_cls = int(caixa.cls[0])
        deteccoes.append({
            'classe': resultado.names[id_cls],
            'confianca': round(float(caixa.conf[0]), 3),
            'coordenadas': [int(v) for v in caixa.xyxy[0]]
        })
    return deteccoes


def contar_objetos(deteccoes, classe):
    """Conta as detecções de uma classe específica."""
    return sum(1 for obj in deteccoes if obj['classe'] == classe)


def verificar_condicao(observacao):
    """
    Aplica a regra do cenário: compara a contagem anterior com a atual
    e verifica se houve mudança relevante na classe monitorada.
    """
    antes = observacao.get('contagem_antes', 0)
    depois = observacao.get('contagem_depois', 0)
    classe = observacao.get('classe_monitorada', 'person')

    mudou = depois != antes
    diferenca = depois - antes

    return {
        'classe': classe,
        'mudanca_detectada': mudou,
        'diferenca': diferenca,
        'alerta_necessario': mudou and depois > 0,
        'resumo': f"Classe '{classe}': antes={antes}, agora={depois} (variação: {diferenca:+d})."
    }


def enviar_alerta(mensagem, token_bot='', chat_id='', caminho_foto=''):
    """Envia mensagem de texto (ou foto anotada com legenda) via Telegram Bot API."""
    if not token_bot or not chat_id:
        print(f'[SIMULAÇÃO TELEGRAM] Alerta disparado:\n👉 {mensagem}')
        if caminho_foto:
            print(f'📷 Foto anotada salva em: {caminho_foto}')
        return {'ok': True, 'simulado': True}

    # Se houver imagem anotada, envia a foto com a mensagem como legenda (sendPhoto)
    if caminho_foto:
        url = f'https://api.telegram.org/bot{token_bot}/sendPhoto'
        with open(caminho_foto, 'rb') as foto:
            resposta = requests.post(
                url,
                data={'chat_id': chat_id, 'caption': mensagem},
                files={'photo': foto},
                timeout=15
            )
        return resposta.json()

    # Caso contrário, envia apenas a mensagem de texto (sendMessage)
    url = f'https://api.telegram.org/bot{token_bot}/sendMessage'
    resposta = requests.post(url, json={'chat_id': chat_id, 'text': mensagem}, timeout=10)
    return resposta.json()
```

---

#### Passo 3: Expondo as Ferramentas em um Servidor MCP

Com o `FastMCP` (parte do SDK oficial do MCP em Python), registramos nossas funções usando o decorador `@mcp_server.tool()`. Repare como as **anotações de tipo** (`str`, `list`, `dict`) e as **docstrings** são importantes: o MCP usa essas informações para criar automaticamente o contrato de cada ferramenta!

```python
from mcp.server.fastmcp import FastMCP

# Cria o servidor MCP do nosso Agente de Visão
mcp_server = FastMCP('VisionAgentServer')

@mcp_server.tool()
def tool_detectar_objetos(caminho_imagem: str, confianca: float = 0.5, salvar_anotada: str = '') -> list:
    """Detecta objetos em uma imagem usando YOLO e retorna classes, confiança e coordenadas."""
    return detectar_objetos(caminho_imagem, limite_confianca=confianca, salvar_anotada=salvar_anotada)

@mcp_server.tool()
def tool_contar_objetos(deteccoes: list, classe: str) -> int:
    """Conta quantos objetos de uma determinada classe existem na lista de detecções."""
    return contar_objetos(deteccoes, classe)

@mcp_server.tool()
def tool_verificar_condicao(observacao: dict) -> dict:
    """Verifica se houve mudança relevante na cena monitorada."""
    return verificar_condicao(observacao)

@mcp_server.tool()
def tool_enviar_alerta(mensagem: str, token_bot: str = '', chat_id: str = '', caminho_foto: str = '') -> dict:
    """Envia um alerta para o Telegram (com texto e foto opcional) caso uma condição crítica seja confirmada."""
    return enviar_alerta(mensagem, token_bot, chat_id, caminho_foto)
```

---

#### Passo 4: Executando o Ciclo Completo do Agente

Agora unimos todas as etapas no ciclo **Observação → Interpretação → Decisão → Ação → Resultado**, salvando também a imagem anotada da cena atual para enviar junto com o alerta:

```python
def executar_vision_agent(
    img_referencia,
    img_atual,
    classe_alvo='person',
    telegram_token='',
    telegram_chat_id=''
):
    foto_evidencia = 'evidencia_yolo.jpg'

    print('1. [OBSERVAÇÃO] Analisando imagens com YOLO...')
    det_antes = tool_detectar_objetos(img_referencia)
    det_depois = tool_detectar_objetos(img_atual, salvar_anotada=foto_evidencia)

    print('2. [INTERPRETAÇÃO] Contabilizando objetos da classe:', classe_alvo)
    qtd_antes = tool_contar_objetos(det_antes, classe_alvo)
    qtd_depois = tool_contar_objetos(det_depois, classe_alvo)

    observacao = {
        'classe_monitorada': classe_alvo,
        'contagem_antes': qtd_antes,
        'contagem_depois': qtd_depois
    }

    print('3. [DECISÃO] Acionando ferramenta de verificação de condição...')
    verificacao = tool_verificar_condicao(observacao)
    print('   Resultado da verificação:', verificacao['resumo'])

    print('4. [AÇÃO] Avaliando regra de alerta...')
    if verificacao['alerta_necessario']:
        msg = (
            f"🚨 ALERTA DO VISION AGENT!\n"
            f"Mudança detectada na cena monitorada.\n"
            f"{verificacao['resumo']}"
        )
        resultado_envio = tool_enviar_alerta(
            mensagem=msg,
            token_bot=telegram_token,
            chat_id=telegram_chat_id,
            caminho_foto=foto_evidencia
        )
        print('5. [RESULTADO] Alerta processado com sucesso!', resultado_envio)
    else:
        print('5. [RESULTADO] Nenhuma anomalia crítica. Nenhum alerta enviado.')


# Teste rápido comparando duas cenas
url_cena_1 = 'https://ultralytics.com/images/zidane.jpg'
url_cena_2 = 'https://ultralytics.com/images/bus.jpg'

executar_vision_agent(
    img_referencia=url_cena_1,
    img_atual=url_cena_2,
    classe_alvo='person'
)
```

> **💡 Dica para o mundo real (Evitando Flood de Alertas em Vídeos):**
> Se você conectar esse agente a uma câmera ao vivo (usando o loop de vídeo do **Dia 02**), uma pessoa caminhando na cena aparecerá em 30 quadros por segundo — o que dispararia 30 mensagens seguidas no Telegram! Em sistemas reais, adicionamos duas regras simples na política do agente:
>
> 1. **Confirmação Temporal (*Debounce*):** Só confirmar a mudança se a nova contagem se mantiver por pelo menos $N$ quadros consecutivos (evita falsos positivos de um único frame borrado).
> 2. **Tempo de Resfriamento (*Cooldown*):** Guardar o horário do último envio (`time.time()`) e aguardar um intervalo mínimo (ex.: 60 segundos) antes de enviar um novo alerta da mesma classe.

<p style="text-align: center; margin-top: 24px;">
<a style="font-size:2vh; background-color: #06e4d9 !important; padding: 10px 18px; border-radius: 6px; color: #000; font-weight: bold; text-decoration: none;" href="https://google.com" target="_blank">Clique aqui para abrir o projeto no Colab</a>
</p>

---

### 💭 Para ir mais a fundo...

---

#### 📚 Materiais e artigos sugeridos

* <a href="https://arxiv.org/html/2506.12469v1" target="_blank">Levels of Autonomy for AI Agents — Working Paper (arXiv)</a>
* <a href="https://modelcontextprotocol.io/" target="_blank">Documentação Oficial do Model Context Protocol (MCP)</a>
* <a href="https://core.telegram.org/bots/api" target="_blank">Documentação Oficial da Telegram Bot API</a>

#### 🤖 Prompts para estudar com LLMs

Copie e cole os prompts abaixo no seu assistente de IA favorito para aprofundar os conceitos vistos na aula:

1. **Arquitetura de Agentes de Visão (Regras vs. LLMs):**
   > *"Explique a diferença entre um pipeline tradicional de Visão Computacional (que apenas executa um modelo como YOLO) e um Agente de Visão (Vision Agent) que segue o ciclo Observação → Interpretação → Decisão → Ação. Compare quando vale a pena usar uma política de decisão baseada em regras determinísticas em Python e quando vale a pena usar um LLM como planejador."*

2. **Model Context Protocol (MCP) na Prática:**
   > *"Explique de forma didática o que é o Model Context Protocol (MCP), qual problema ele resolve no desenvolvimento de agentes de IA e qual é o papel do Host, do Cliente MCP e do Servidor MCP. Mostre um exemplo simples em Python usando FastMCP expondo duas ferramentas."*

3. **Integrando Visão Computacional com Bot do Telegram:**
   > *"Como criar um bot no Telegram usando o @BotFather, obter o Token e o Chat ID, e enviar tanto mensagens de texto quanto imagens anotadas pelo OpenCV/YOLO em Python usando a biblioteca requests? Mostre o código passo a passo e explique boas práticas para evitar enviar múltiplos alertas repetidos em vídeos."*
