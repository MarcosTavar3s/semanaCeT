# Fundamentos de imagens digitais

---

### Ferramentas:

---

<div style="inline-flex">
<a href="https://colab.new" target="_blank"><img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Google Colab"></a>
<img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" alt="OpenCV">
<img src="https://img.shields.io/badge/Pillow-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Pillow">
<img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" alt="NumPy">
</div>

---

### Definições essenciais 
---
<p>
Toda imagem é composta por pixels (picture element), a menor unidade de representação visual computacional. Assim, <strong>a imagem pode ser descrita como um conjunto de valores numéricos (pixels) organizados em uma matriz.</strong> Nesse sentido, para aprendermos a manipular imagens, vamos conhecer sobre a definição de resolução, escala de cores e canais.
</p>

<img src="/assets/images/pixels.jpeg" alt="Imagem didática dividida em duas partes com fundo de um recife de corais desfocado. À esquerda, há um painel rotulado Imagem Original de Alta Resolução que exibe uma cena subaquática nítida com corais coloridos, um peixe e bolhas de ar, destacando uma área delimitada por um quadrado branco. Uma seta aponta desse quadrado para a direita, indicando a extração do elemento. À direita, vê-se uma amostra ampliada de um pixel de cor azul sólido, acompanhada pelo texto descritivo. Amostra do Pixel Extraído e uma pequena representação de uma grade de pixels (Local de Origem - Mapa de Bits 1x1)pontando exatamente de onde ele veio.">

- **Resolução:** consiste nas dimensões da imagem, ou seja, a largura e a altura. Ela representa a quantidade total de pixels organizada espacialmente na matriz.
    
    - Maior resolução -> Maior quantidade de pixels -> Maior quantidade de detalhes representados na imagem. Ex: Full HD (1920 x 1080 pixels) versus Ultra HD 4K (3840 x 2160 pixels). Qual é mais rica em detalhes?

- **Canais e escala de cores:** os canais representam a quantidade de camadas que compõe a imagem. Eles se relacionam com as propriedades de cores, luz, transparência em cada pixel. Através deles, é estabelecido as escalas de cores. Dentre as mais famosas, temos:

=== "RGB"

    * **Canais:** **R** (Red), **G** (Green), **B** (Blue)
    * **Descrição / Aplicação:** Telas, monitores e fotografia digital.

=== "CMYK"

    * **Canais:** **C** (Cyan), **M** (Magenta), **Y** (Yellow), **K** (Black)
    * **Descrição / Aplicação:** Impressão gráfica e editorial.

=== "HSV / HSB"

    * **Canais:** **H** (Hue), **S** (Saturation), **V/B** (Value/Brightness)
    * **Descrição / Aplicação:** Seletores de cor em softwares de design.

=== "HSL"

    * **Canais:** **H** (Hue), **S** (Saturation), **L** (Lightness)
    * **Descrição / Aplicação:** Desenvolvimento web (CSS) e UI/UX.

=== "Grayscale"

    * **Canais:** **L** (Luminance / Intensidade)
    * **Descrição / Aplicação:** Canal único. Fotografia P&B e processamento de imagem.

=== "CIE Lab"

    * **Canais:** **L** (Lightness), **a** (Verde-Vermelho), **b** (Azul-Amarelo)
    * **Descrição / Aplicação:** Gerenciamento profissional de cor.

=== "RGBA / HSLA"

    * **Canais:** Canais base + **A** (Alpha)
    * **Descrição / Aplicação:** Extensão com controle de opacidade/transparência.

=== "YUV / YCbCr"

    * **Canais:** **Y** (Luma), **U/Cb** (Chroma Azul), **V/Cr** (Chroma Vermelho)
    * **Descrição / Aplicação:** Separa luminosidade da cor. Transmissão e compressão de vídeo/JPEG.


**Observação:** maior quantidade de canais -> maior quantidade de informações -> maior demanda de memória. 


- **Filtros:** operações realizadas para modificar as características da imagem. São usados para atenuar, acentuar, detectar bordas, entre outras.
=== "Filtros Espaciais"

    Utilizados principalmente em suavização e aguçamento de imagens. Modifica o valor do pixel com base na sua vizinhança.

    * **Desenfoque Gaussiano (Gaussian Blur):** Aplica uma matriz baseada na distribuição gaussiana para suavizar a imagem e reduzir ruídos de alta frequência.
    * **Filtro de Médias / Mediana:** Substitui o valor do pixel pela média ou mediana dos vizinhos. A mediana é ideal para remover ruído do tipo "sal e pimenta".
    * **Nitidez (Unsharp Mask / Laplaciano):** Realça as bordas e detalhes finos aumentando o contraste entre pixels adjacentes de alta frequência.

=== "Filtros de Detecção de Bordas"

    * **Sobel:** Calcula o gradiente da intensidade da imagem em duas direções (X e Y) para destacar transições abruptas de cor e brilho.
    * **Canny:** Algoritmo multi-estágio extremamente preciso para detecção de contornos com supressão de não-máximos e limiarização por histerese.
    * **Prewitt:** Semelhante ao Sobel, opera com máscaras de convolução para estimar a magnitude e orientação de bordas.

=== "Filtros de Ajuste Tonal & Cor"

    * **Brilho e Contraste:** Ajusta linearmente a intensidade dos pixels (brilho) e a amplitude da diferença entre tons claros e escuros (contraste).
    * **Equalização de Histograma:** Distribui as intensidades de cinza de forma homogênea para maximizar o contraste global da imagem.
    * **Ajuste de Gamma (Correção Gamma):** Mapeia a luminância usando uma função de potência não linear para corrigir a percepção visual ou exposição.
    * **Matiz / Saturação (HSV Shift):** Altera a roda de cores (Matiz) ou a intensidade da vivacidade das cores (Saturação).

=== "Filtros Morfológicos"

    * **Erosão:** Encolhe as regiões claras da imagem, removendo pequenos ruídos e desconectando objetos finos.
    * **Dilatação:** Expande as regiões claras da imagem, preenchendo pequenos buracos e conectando elementos próximos.
    * **Abertura / Fechamento:** Combinações de erosão e dilatação usadas para isolar ou agrupar estruturas em imagens binárias.

=== "Filtros de Estilização & Efeitos"

    * **Pequeno Detalhe / Emboss (Relevo):** Cria um efeito tridimensional simulação de sombra e iluminação ao longo das bordas.
    * **Vetorização / Posterizar:** Reduz o número de cores únicas da imagem, agrupando gradientes em blocos de cor sólida.
    * **Vinheta:** Escurece gradualmente as bordas externas da imagem para focar a atenção no centro.

- **Máscaras:** pequenas matrizes ou imagens usadas para modificar uma região. Utilzadas para destacar uma região dentro de uma imagem maior.

<img src="/assets/images/mascara.jpeg" alt="" style="display:flex; justify-text:center">



---
### Práticas propostas

---

1. Abrir uma imagem, checar a resolução e a quantidade de canais.
2. B

---
### 💭 Para ir mais a fundo....
---
#### Artigos sugeridos:



#### Prompts para utilizar em LLMs:

<div class="divs">

Explique como uma imagem digital monocromática e uma colorida são representadas matematicamente na memória (como matrizes/tensores). Aborde os conceitos de amostragem espacial (resolução espacial) e quantização de intensidade (resolução em tons de cinza ou profundidade de bits), detalhando o impacto de cada um na perda ou preservação de detalhes visuais.

<br/><br/>

Explique o funcionamento dos canais de cor em imagens digitais. Compare o espaço de cores RGB com o HSV (ou HSL) e o CIELAB, explicando em quais cenários de visão computacional ou tratamento de imagem é mais vantajoso converter uma imagem de RGB para HSV ou para tons de cinza (grayscale).

<br/><br/>

Explique o conceito de máscaras binárias e operações morfológicas básicas em processamento de imagens (Erosão, Dilatação, Abertura e Fechamento). 

<br/><br/>

Explique intuitivamente e matematicamente como cada uma dessas quatro operações altera os pixels de uma imagem binarizada e dê exemplos clássicos de uso.

</div>