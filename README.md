# HandTrackingModule
Claro! Aqui está uma documentação básica no estilo Markdown para o arquivo `main.py` que você forneceu:

```markdown
# Detecção de Mãos usando OpenCV e MediaPipe

Este é um script Python para detectar mãos em tempo real usando a biblioteca OpenCV e a biblioteca MediaPipe.

## Pré-requisitos

Certifique-se de ter as seguintes bibliotecas instaladas:

- OpenCV
- MediaPipe

Você pode instalar essas bibliotecas usando o seguinte comando:

```bash
pip install opencv-python mediapipe
```

## Uso

1. Importe as bibliotecas necessárias:

```python
import cv2
import mediapipe as mp
```

2. Inicialize a captura de vídeo e as ferramentas MediaPipe:

```python
cap = cv2.VideoCapture(0)
mpHands = mp.solutions.hands
hands = mpHands.Hands()
mpDraw = mp.solutions.drawing_utils
```

3. Inicie o loop principal para processar cada quadro de vídeo:

```python
while True:
    success, image = cap.read()
    imageRGB = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
    results = hands.process(imageRGB)

    if results.multi_hand_landmarks:
        for handLms in results.multi_hand_landmarks: 
            for id, lm in enumerate(handLms.landmark):
                h, w, c = image.shape
                cx, cy = int(lm.x * w), int(lm.y * h)

                if id == 20:
                    cv2.circle(image, (cx, cy), 25, (255, 0, 255), cv2.FILLED)
                    mpDraw.draw_landmarks(image, handLms, mpHands.HAND_CONNECTIONS)

    cv2.imshow("Output", image)
    cv2.waitKey(1)
```

## Explicação

1. O código começa inicializando a captura de vídeo e as ferramentas do MediaPipe para detecção de mãos.

2. O loop principal captura quadros de vídeo da webcam.

3. Os quadros são convertidos para o formato RGB e processados ​​para detectar mãos usando o MediaPipe.

4. Se mãos forem detectadas, o código itera sobre os pontos de referência da mão e desenha um círculo no ponto correspondente ao dedo médio.

5. Os pontos de referência da mão e as conexões entre eles são desenhados no quadro.

6. O quadro resultante é exibido usando a OpenCV.

7. O loop continua até que o usuário pressione a tecla "Esc" ou feche a janela de saída.

Certifique-se de encerrar a captura de vídeo e liberar os recursos após terminar a execução do código.

## Créditos

Este script foi criado com base no tutorial de detecção de mãos usando MediaPipe e OpenCV.

[Link para o tutorial](https://google.github.io/mediapipe/solutions/hands)
```

Certifique-se de substituir os detalhes no documento, como os pré-requisitos e os créditos, de acordo com suas necessidades.
