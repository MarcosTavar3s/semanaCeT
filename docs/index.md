# Bem-vind@ ao BootCamp de Visão Computacional + IA na prática

--- 

BootCamp na Semana de C&T: 10, 11 e 12 de Novembro de 2026 - Desenvolvido por Marcos Aurélio Tavares Filho.

---
## Cronograma
---

#### [`Dia 01:` Fundamentos de Imagens Digitais](dia01.md)
* O que é uma imagem digital: representação matricial (pixels) e resolução ($\text{altura} \times \text{largura}$);
* Canais e escalas de cores (`RGB`/`BGR`, `HSV`/`HSB`, `Grayscale`, `CMYK`, `HSL`, `CIE Lab`, `RGBA`/`HSLA` e `YUV`/`YCbCr`);
* Ajustes tonais e de cor (brilho, contraste, saturação, histograma e gamma), filtros espaciais (suavização e nitidez), detecção de bordas (`Sobel`, `Canny` e `Prewitt`), filtros morfológicos, efeitos e máscaras (`ROI` e `Kernel`);
* **Prática:** Construção de um **Editor de Imagens Interativo** em Python no Google Colab com OpenCV, NumPy, Matplotlib e `ipywidgets`.


#### [`Dia 02:` Modelos de Visão Computacional e YOLO](dia02.md)
* Dos filtros manuais ao aprendizado automático: antes vs. depois das Redes Neurais Convolucionais (CNNs), visão em camadas e simulador interativo de convolução (`Kernel`, `ReLU` e `Feature Map`);
* As 4 tarefas principais da visão computacional (Classificação, Detecção de Objetos, Segmentação e Estimativa de Pose);
* O que é o **YOLO** (*You Only Look Once*), como ele funciona passo a passo (*Letterbox*, *Grid*, palpites simultâneos, filtro de confiança, *IoU* e *NMS*), base **COCO** ($80$ classes) e tamanhos do modelo (`n`, `s`, `m`, `l`, `x`);
* Como ensinar novos objetos ao YOLO: *Datasets* (Kaggle e Roboflow), anotação de dados (*Data Labeling*), estrutura `data.yaml` e ajuste fino (*Fine-Tuning*);
* **Prática:** Detecção de objetos, extração estruturada de dados, segmentação de instâncias, estimativa de pose e processamento de vídeos (*frame a frame*) no Google Colab com **Ultralytics YOLO + OpenCV**.

#### [`Dia 03:` Agentes + Visão Computacional](dia03.md)
* Conceito de Agentes de IA e Agentes de Visão (*Vision Agent*) no ciclo *Observação → Interpretação → Decisão → Ação → Resultado*;
* Diferença entre Bot e Agente e os 5 níveis de autonomia de agentes de IA (`L1` a `L5`);
* Padronização e integração de ferramentas (*tools*) com o **Model Context Protocol (MCP)** (*Host*, *Cliente MCP* e *Servidor MCP*);
* Reflexão filosófica sobre consciência na IA com o **Experimento da Sala Chinesa** de John Searle (*Sintaxe vs. Semântica* / *IA Fraca vs. IA Forte*) e próximos passos na **ECT/UFRN**;
* **Prática:** Construção de um **Vision Agent** para detectar mudanças entre cenas no Google Colab com **YOLO + OpenCV**, Servidor MCP (`FastMCP`), política de decisão (regras determinísticas e expansão com LLM) e envio de alertas automáticos com foto via **Telegram Bot API**.

---
## Equipe
---

* Professor responsável: [Helton Maia](https://heltonmaia.com/)
* Ministrante: [Marcos Aurélio Tavares Filho (Engenharia de Computação)](https://www.linkedin.com/in/marcos-aurelio-tavares-filho)
* Monitores: [João Victor Evangelista (Engenharia de Computação)](https://www.linkedin.com/in/jo%C3%A3o-vitor-evangelista-b22a73180/) e Sara Thamyres (Ciência e Tecnologia)
