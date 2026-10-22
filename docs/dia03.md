# Agentes + Visão Computacional 

---
### Agente (IA)
---

Observa um ambiente, processa informações e escolhe ações para alcançar um objetivo. Um ciclo simplificado é:
        
    Observação → Interpretação → Decisão → Ação → Resultado

O agente pode combinar modelos de aprendizado de máquina, regras e funções Python. Nem todo agente precisa usar um modelo de linguagem.

- Agente de visão: utiliza imagens ou vídeos como observações do ambiente. Um detector convencional retorna objetos identificados; um agente pode usar essas detecções para escolher verificações, executar ferramentas e decidir o próximo passo.

---
### Bot e agente: qual é a diferença?
---

Um **bot** automatiza interações ou tarefas, geralmente por comandos ou regras. Um **agente** seleciona ações de acordo com o objetivo e as observações disponíveis. Os conceitos não são excludentes: um bot pode fazer parte de um sistema agente.

<img src="/assets/images/bot_vs_agente_recife.png" alt="">


---

### MCP

---

O **Model Context Protocol (MCP)** é um protocolo aberto para que aplicações de IA descubram e utilizem ferramentas e recursos disponibilizados por servidores compatíveis.

- **Host:** aplicação que integra o modelo e coordena a interação.
- **Cliente MCP:** conecta-se ao servidor e solicita operações.
- **Servidor MCP:** expõe ferramentas, como funções Python.

MCP não é um modelo de IA nem escolhe sozinho as ferramentas: a decisão cabe ao agente ou ao código que o coordena. No Colab, cliente e servidor podem ser testados na mesma sessão, usando um transporte compatível, como o transporte em memória do SDK.

--- 

### Níveis de autonomia de agentes de IA

---
<img src="/assets/images/niveis_autonomia_agentes_ia.png" alt="">



<p style="text-align: center;"> Artigo original: <a href="https://arxiv.org/html/2506.12469v1" target="_blank">Levels of Autonomy for AI Agents Working Paper.</a></p>

---

### Projeto: Vision Agent para detectar mudanças entre imagens

---

```text
Imagem enviada ao Colab
        ↓
YOLO + OpenCV
        ↓
Detecções estruturadas
        ↓
Política de decisão do agente
        ↓
Cliente MCP ↔ Servidor MCP
        ↓
Ferramenta de verificação
        ↓
Regra de alerta
        ↓
Telegram Bot API
```

#### Ferramentas do agente
 
- `detectar_objetos(imagem)`: retorna classes, confiança e coordenadas.
- `contar_objetos(deteccoes, classe)`: conta as detecções de uma classe.
- `verificar_condicao(observacao)`: aplica uma regra definida para o cenário.
- `enviar_alerta(mensagem, chat_id)`: envia uma mensagem ao Telegram.

#### Como o agente escolhe o que investigar?

A versão básica usa regras determinísticas. Por exemplo, se forem detectadas pessoas, o agente pode escolher uma verificação adicional. Em seguida, a regra de alerta avalia o resultado e decide se deve notificar.

Um modelo de linguagem pode substituir ou complementar esse planejador, selecionando ferramentas com base no objetivo e nos resultados anteriores. Essa opção pode exigir acesso a um provedor de modelo e uma chave de API; por isso, não é necessária para a versão básica do minicurso.

<a style="font-size:2vh; background-color: #06e4d9 !important;" href="https://google.com" target="_blank">Clique aqui para abrir o projeto no Colab</a>