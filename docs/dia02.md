# Modelos de Visão Computacional

---

### 🛠️ Ferramentas Utilizadas

---

<div style="inline-flex">
<a href="https://colab.new" target="_blank"><img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Google Colab"></a>
<img src="https://img.shields.io/badge/Ultralytics%20YOLO-111F68?style=for-the-badge&logo=yolo&logoColor=white" alt="Ultralytics YOLO">
<img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" alt="OpenCV">
<img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" alt="PyTorch">
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
</div>

---

### 🧠 1. Do Pixel ao Neurônio Artificial

---

No **Dia 01**, vimos que uma imagem digital é uma matriz de números (pixels) e que podemos aplicar **filtros manuais** (como Sobel ou Canny) para destacar bordas. Mas como ensinar o computador a reconhecer padrões complexos — como diferenciar um cachorro de um gato, ou identificar uma pessoa em uma câmera de segurança — sem programar regras manuais para cada formato existente?

A resposta está nas **Redes Neurais Artificiais**, cuja unidade fundamental é o **neurônio artificial** (ou *Perceptron*).

```text
Entradas (Pixels)        Pesos (Importância)         Soma + Bias        Função de Ativação
   x₁ (ex: 120)  ───────►   w₁                     ┌─────────────┐       ┌──────────────┐
   x₂ (ex: 255)  ───────►   w₂   ───────────────►  │ z = ∑(x·w)+b│ ────► │ Saída: f(z)  │
   x₃ (ex: 40)   ───────►   w₃                     └─────────────┘       └──────────────┘
```

#### As 4 etapas de um neurônio:

1. **Entradas ($x$):** Os valores numéricos que alimentam o neurônio (por exemplo, a intensidade dos pixels ou características extraídas da imagem).
2. **Pesos ($w$ - *Weights*) e Viés ($b$ - *Bias*):**
   * Cada entrada é multiplicada por um **peso ($w$)**, que define o quão importante aquela informação é para a decisão.
   * O **viés ($b$)** funciona como um limiar de ajuste, facilitando ou dificultando o disparo do neurônio.
3. **Soma Ponderada ($z$):** O neurônio combina tudo na equação: $z = (x_1 \cdot w_1 + x_2 \cdot w_2 + \dots) + b$.
4. **Função de Ativação ($f(z)$):** Decide qual sinal será passado adiante e introduz **não-linearidade** (permitindo aprender padrões complexos). A mais usada em visão computacional é a **ReLU** (*Rectified Linear Unit*), que simplesmente zera valores negativos e mantém os positivos: $f(z) = \max(0, z)$.

> **Ideia central:** Aprender em uma rede neural significa **ajustar automaticamente os pesos ($w$) e vieses ($b$)** a partir de exemplos, até que a rede acerte a tarefa desejada.

#### Por que uma rede comum (MLP) não basta para imagens?

Se tentarmos conectar todos os pixels de uma imagem Full HD ($1920 \times 1080 \times 3 \approx 6{,}2 \text{ milhões de valores}$) diretamente em neurônios tradicionais:

* **Explosão de parâmetros:** Um único neurônio precisaria de 6,2 milhões de pesos! Uma camada com mil neurônios teria mais de 6 bilhões de conexões.
* **Perda da estrutura espacial:** Ao "achatar" a matriz da imagem em uma fila única de números, o neurônio perde a noção de quais pixels eram vizinhos (acima, abaixo, ao lado).

---

### 🔍 2. Redes Neurais Convolucionais (CNNs)

---

Para resolver o problema das imagens, surgiram as **CNNs** (*Convolutional Neural Networks* ou Redes Neurais Convolucionais).

Lembra das **máscaras de convolução (kernels $3\times3$)** do Dia 01, que deslizavam pela imagem para detectar bordas ou aplicar nitidez?

<p style="text-align:center"><strong>Em uma CNN, os filtros (kernels) são os próprios pesos dos neurônios!</strong> Em vez de um humano escolher os números do filtro manualmente, <strong>a própria rede aprende quais filtros são melhores</strong> durante o treinamento.</p>

```text
Imagem de Entrada ──► [ Convolução + ReLU ] ──► [ Pooling ] ──► [ Camadas Densa/Final ] ──► Predição
   (Matriz RGB)       (Extrai características)  (Reduz tamanho)   (Combina pistas)        ("Carro: 96%")
```

#### Os 3 blocos fundamentais de uma CNN:

=== "1. Camada de Convolução"

    * Pequenos filtros (ex.: $3\times3$) deslizam pela imagem multiplicando os pixels locais.
    * Cada filtro gera um **Mapa de Características (*Feature Map*)**, destacando onde determinado padrão visual apareceu na imagem.
    * **Compartilhamento de pesos:** O mesmo filtro percorre a imagem inteira. Se ele aprende a detectar uma roda no canto esquerdo, também saberá detectá-la no canto direito, economizando milhões de parâmetros.

=== "2. Camada de Pooling (Agrupamento)"

    * Reduz a resolução espacial dos mapas de características (por exemplo, o **Max Pooling $2\times2$** pega apenas o maior valor de cada bloco de 4 pixels).
    * **Objetivo:** Diminuir o custo computacional e tornar a rede resistente a pequenas variações de posição ou rotação do objeto.

=== "3. Hierarquia de Características"

    À medida que a imagem avança pelas camadas da CNN, o nível de abstração aumenta:

    * **Camadas iniciais:** Aprendem **bordas**, linhas horizontais/verticais e manchas de cor.
    * **Camadas intermediárias:** Combinam bordas para formar **texturas e partes** (olhos, rodas, cantos, folhas).
    * **Camadas profundas:** Combinam partes para reconhecer **objetos completos** (rostos, carros, animais).

---

### 🎯 3. Tarefas da Visão Computacional

---

Com base nas características extraídas pelas CNNs, os modelos modernos de visão computacional podem realizar diferentes tarefas dependendo do que precisamos responder sobre a cena:

=== "Classificação"

    * **Pergunta:** *"O que existe nesta imagem?"*
    * **Saída:** Um rótulo global (classe) e sua probabilidade para a imagem inteira.
    * **Exemplo:** Classificar uma foto médica ou identificar se uma imagem contém um `cachorro` ($98\%$).

=== "Detecção de Objetos"

    * **Pergunta:** *"O que existe na imagem e **onde** cada objeto está?"*
    * **Saída:** Lista de objetos contendo **Classe**, **Confiança (0 a 1)** e **Caixa Delimitadora (*Bounding Box*)** com as coordenadas $(x_1, y_1, x_2, y_2)$.
    * **Exemplo:** Contar pessoas, veículos e semáforos em uma rua.

=== "Segmentação de Instâncias"

    * **Pergunta:** *"Quais são os pixels exatos de cada objeto?"*
    * **Saída:** Além da caixa e da classe, gera uma **máscara binária** contornando exatamente o formato de cada objeto individual.
    * **Exemplo:** Recorte preciso de objetos, direção autônoma e análise de áreas agrícolas.

=== "Estimativa de Pose (Keypoints)"

    * **Pergunta:** *"Qual é a postura ou estrutura articulada do objeto?"*
    * **Saída:** Coordenadas de **pontos-chave (*keypoints*)** conectados (como ombros, cotovelos, joelhos).
    * **Exemplo:** Análise de movimentos esportivos, ergonomia e detecção de quedas.

---

### ⚡ 4. YOLO: Detecção de Imagens em Tempo Real

---

Antigamente, para detectar objetos em uma imagem, os algoritmos funcionavam em **duas etapas lentas**: primeiro recortavam milhares de regiões candidatas na imagem e depois passavam uma CNN classificadora em cada recorte. Isso inviabilizava o uso em vídeos ao vivo.

Em 2016, surgiu o **YOLO** (*You Only Look Once* — *"Você Só Olha Uma Vez"*), revolucionando a área ao transformar a detecção em uma **única passagem pela rede neural**.

#### Como o YOLO pensa? (Ideia Central Simplificada)

```text
+---+---+---+
|   |   |   |   1. Divide a imagem em uma grade (Grid S × S)
+---+---+---+   2. Cada célula prevê caixas (x, y, largura, altura),
|   | ● |   |      confiança e probabilidades de classe simultaneamente!
+---+---+---+   3. O NMS limpa as caixas duplicadas sobre o mesmo objeto.
|   |   |   |
+---+---+---+
```

1. **Divisão em Grade (*Grid*):** O YOLO divide a imagem de entrada em uma grade (por exemplo, $S \times S$ células).
2. **Previsão Simultânea:** Se o centro de um objeto cai dentro de uma célula da grade, aquela célula fica responsável por prever:
   * As coordenadas da **Bounding Box**: centro $(x, y)$, largura $(w)$ e altura $(h)$;
   * O **grau de confiança (*Confidence*)**: a certeza de que há um objeto ali;
   * As **probabilidades de cada classe** (pessoa, carro, mochila, celular etc.).
3. **Remoção de Duplicatas — NMS (*Non-Maximum Suppression*):**
   * Como células vizinhas podem detectar o mesmo objeto, várias caixas acabam se sobrepondo.
   * O YOLO calcula a métrica **IoU (*Intersection over Union*)**, que mede a área de sobreposição entre duas caixas.
   * O algoritmo **NMS** mantém apenas a caixa com **maior confiança** e descarta as caixas redundantes que estão muito sobrepostas a ela.

```text
Antes do NMS:                      Depois do NMS:
┌─────────────┐                    ┌─────────────┐
│ ┌───────────┼─┐                  │             │
│ │ Pessoa 92%│ │  ──(IoU alto)──► │ Pessoa 92%  │
└─┼───────────┘ │                  │             │
  └─────────────┘                  └─────────────┘
 (Várias caixas)                   (Apenas a melhor)
```

> **Dica prática:** Modelos YOLO pré-treinados (como o **YOLOv8 / YOLO11** da biblioteca `ultralytics`) já vêm treinados na base **COCO**, sendo capazes de reconhecer **80 classes do dia a dia** instantaneamente (pessoas, carros, animais, utensílios etc.), além de suportarem classificação, detecção, segmentação e pose!

---

### 💻 5. Prática: Identificando Objetos em Imagens e Vídeos com YOLO

---

Nesta prática, vamos utilizar a biblioteca **Ultralytics YOLO** integrada ao **OpenCV** para:

- Carregar um modelo YOLO pré-treinado;
- Identificar objetos e extrair **classes, confiança e coordenadas** (base para o nosso Agente do **Dia 03**!);
- Testar **Detecção**, **Segmentação** e **Estimativa de Pose**;
- Processar quadros (*frames*) de vídeo / webcam.

---

#### Passo 1: Instalação e Carregamento do Modelo

No Google Colab, instalamos o pacote `ultralytics` e carregamos o modelo `yolov8n.pt` (versão *Nano*, super leve e rápida).

```python
# No Google Colab, descomente a linha abaixo para instalar:
# !pip install -q ultralytics opencv-python matplotlib

import cv2
import matplotlib.pyplot as plt
from ultralytics import YOLO

# Carrega o modelo YOLO pré-treinado (baixa os pesos automaticamente na 1ª vez)
modelo = YOLO('yolov8n.pt')

print(f'Quantidade de classes conhecidas pelo modelo: {len(modelo.names)}')
print('Exemplos de classes:', list(modelo.names.values())[:10])
```

---

#### Passo 2: Detectando Objetos e Inspecionando Resultados

Vamos passar uma imagem para o modelo, visualizar as caixas desenhadas e inspecionar os dados estruturados de cada objeto detectado.

```python
# Podemos passar o caminho de um arquivo local ou uma URL de imagem
url_imagem = 'https://ultralytics.com/images/bus.jpg'

# Realiza a predição (conf=0.40 ignora detecções com menos de 40% de certeza)
resultados = modelo.predict(source=url_imagem, conf=0.40)
resultado = resultados[0]

# Extrai a imagem anotada com as caixas (vem em BGR, convertemos para RGB)
img_anotada = cv2.cvtColor(resultado.plot(), cv2.COLOR_BGR2RGB)

plt.figure(figsize=(8, 10))
plt.title('Detecção de Objetos com YOLO')
plt.imshow(img_anotada)
plt.axis('off')
plt.show()

# Inspecionando cada detecção individualmente
print('Objetos Detectados ' + '=' * 35)
for caixa in resultado.boxes:
    id_classe = int(caixa.cls[0])
    nome_classe = resultado.names[id_classe]
    confianca = float(caixa.conf[0])
    x1, y1, x2, y2 = map(int, caixa.xyxy[0])

    print(f'• Classe: {nome_classe:<10} | Confiança: {confianca:.1%} | Caixa: ({x1}, {y1}) até ({x2}, {y2})')
```

---

#### Passo 3: Transformando em Função Estruturada (Ponte para o Dia 03)

No **Dia 03**, nosso Agente de Visão precisará receber as detecções como uma estrutura de dados limpa em Python para tomar decisões e contar objetos. Veja como é simples encapsular o YOLO:

```python
def detectar_objetos(caminho_ou_img, limite_confianca=0.5):
    """
    Executa o YOLO na imagem e retorna uma lista de dicionários
    com classe, confiança e coordenadas da bounding box.
    """
    resultados = modelo.predict(source=caminho_ou_img, conf=limite_confianca, verbose=False)
    deteccoes = []

    for caixa in resultados[0].boxes:
        id_cls = int(caixa.cls[0])
        deteccoes.append({
            'classe': resultados[0].names[id_cls],
            'confianca': round(float(caixa.conf[0]), 3),
            'coordenadas': [int(v) for v in caixa.xyxy[0]]  # [x1, y1, x2, y2]
        })

    return deteccoes


def contar_objetos(deteccoes, classe_alvo):
    """Conta quantos objetos de uma classe específica foram detectados."""
    return sum(1 for obj in deteccoes if obj['classe'] == classe_alvo)


# Testando nossas funções
lista_detectada = detectar_objetos(url_imagem, limite_confianca=0.5)
total_pessoas = contar_objetos(lista_detectada, 'person')

print(f'Total de objetos encontrados: {len(lista_detectada)}')
print(f'Total de pessoas ("person"):  {total_pessoas}')
```

---

#### Passo 4: Muito além de Caixas — Segmentação e Pose

Na biblioteca Ultralytics, a mesma estrutura de código serve para **Segmentação de Instâncias** (máscaras por objeto) e **Estimativa de Pose** (esqueleto/keypoints), bastando trocar o sufixo do arquivo do modelo:

* `yolov8n.pt` → Detecção (Caixas)
* `yolov8n-seg.pt` → Segmentação (Máscaras)
* `yolov8n-pose.pt` → Pose (Pontos-chave do corpo)

```python
# Carregando modelos de Segmentação e Pose
modelo_seg = YOLO('yolov8n-seg.pt')
modelo_pose = YOLO('yolov8n-pose.pt')

res_seg = modelo_seg.predict(source=url_imagem, conf=0.40, verbose=False)[0]
res_pose = modelo_pose.predict(source=url_imagem, conf=0.40, verbose=False)[0]

# Comparando lado a lado
fig, ax = plt.subplots(1, 2, figsize=(14, 8))

ax[0].set_title('Segmentação de Instâncias (yolov8n-seg)')
ax[0].imshow(cv2.cvtColor(res_seg.plot(), cv2.COLOR_BGR2RGB))
ax[0].axis('off')

ax[1].set_title('Estimativa de Pose / Keypoints (yolov8n-pose)')
ax[1].imshow(cv2.cvtColor(res_pose.plot(), cv2.COLOR_BGR2RGB))
ax[1].axis('off')

plt.tight_layout()
plt.show()
```

---

#### Passo 5: Processamento de Vídeos (Frame a Frame)

Um **vídeo** nada mais é do que uma sequência de imagens (*frames*) exibidas rapidamente ao longo do tempo (ex.: 30 quadros por segundo). Com o `cv2.VideoCapture`, lemos um quadro de cada vez, passamos pelo YOLO e salvamos o vídeo anotado:

```python
def processar_video_yolo(caminho_entrada, caminho_saida='video_detectado.mp4', max_frames=150):
    cap = cv2.VideoCapture(caminho_entrada)

    # Propriedades do vídeo original
    largura = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    altura = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
    fps = cap.get(cv2.CAP_PROP_FPS) or 30.0

    # Configura o gravador de vídeo de saída
    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    saida = cv2.VideoWriter(caminho_saida, fourcc, fps, (largura, altura))

    frame_atual = 0
    while cap.isOpened() and frame_atual < max_frames:
        sucesso, frame_bgr = cap.read()
        if not sucesso:
            break

        # Executa o YOLO no frame atual e desenha as detecções
        resultado = modelo.predict(source=frame_bgr, conf=0.40, verbose=False)[0]
        frame_anotado = resultado.plot()

        saida.write(frame_anotado)
        frame_atual += 1

    cap.release()
    saida.release()
    print(f'Processamento concluído! {frame_atual} frames salvos em: {caminho_saida}')
```

---

### 💭 Para ir mais a fundo...

---

#### 📚 Materiais e ferramentas interativas sugeridas

* <a href="https://poloclub.github.io/cnn-explainer/" target="_blank">CNN Explainer — Visualização interativa de uma Rede Neural Convolucional funcionando no navegador</a>
* <a href="https://playground.tensorflow.org/" target="_blank">TensorFlow Playground — Simulador visual de neurônios, pesos e funções de ativação</a>
* <a href="https://docs.ultralytics.com/pt/" target="_blank">Documentação Oficial da Ultralytics YOLO (em Português)</a>

#### 🤖 Prompts para estudar com LLMs

Copie e cole os prompts abaixo no seu assistente de IA favorito para aprofundar os conceitos vistos na aula:

1. **Dos Filtros Manuais às CNNs:**
   > *"No estudo de processamento de imagens, aprendi sobre filtros espaciais manuais (como Sobel e Gaussiano) usando matrizes de convolução (kernels). Explique de forma didática como as Redes Neurais Convolucionais (CNNs) aproveitam essa mesma operação de convolução, qual é o papel dos pesos aprendidos, da função de ativação ReLU e da camada de Max Pooling."*

2. **Funcionamento Interno do YOLO, IoU e NMS:**
   > *"Explique passo a passo e sem excesso de jargões matemáticos como o algoritmo YOLO realiza a detecção de objetos em uma única passagem (single-stage detector). Detalhe como funciona a divisão por grade (grid), o que significa a métrica IoU (Intersection over Union) e por que o pós-processamento com Non-Maximum Suppression (NMS) é indispensável."*

3. **Fine-Tuning (Treinando seu Próprio YOLO):**
   > *"Quero treinar um modelo YOLO customizado (fine-tuning) usando Python e a biblioteca Ultralytics para detectar um objeto específico que não existe na base padrão COCO (por exemplo, equipamentos de proteção individual - EPIs). Explique o fluxo completo: quantas imagens coletar, como anotar as caixas delimitadoras (Roboflow/LabelImg), como é o arquivo data.yaml e qual comando Python executa o treinamento."*
