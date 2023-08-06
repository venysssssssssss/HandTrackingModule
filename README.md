# Main.py

```markdown
# Detecção de Mãos usando OpenCV e MediaPipe

Este é um script Python para detectar mãos em tempo real usando a biblioteca OpenCV e a biblioteca MediaPipe.

## Pré-requisitos

Certifique-se de ter as seguintes bibliotecas instaladas:

- OpenCV
- MediaPipe

Você pode instalar essas bibliotecas usando o seguinte comando:

```bash
pip install opencv-python 
pip install mediapipe
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

3. Os quadros são convertidos de BGR para o formato RGB e processados ​​para detectar mãos usando o MediaPipe.

4. Se mãos forem detectadas, o código itera sobre os pontos de referência da mão e desenha um círculo no ponto correspondente ao dedo mindinho.

5. Os pontos de referência da mão e as conexões entre eles são desenhados no quadro.

6. O quadro resultante é exibido usando a OpenCV.

7. O loop continua até o fim da utilização da IDE.


## Créditos

Este script foi criado com base no tutorial de detecção de mãos usando MediaPipe e OpenCV.

[Link para o tutorial](https://google.github.io/mediapipe/solutions/hands)

# HandTrackingModule.py 

```python
import cv2
import mediapipe as mp

class handTracker():
    def __init__(self, mode=False, maxHands=2, detectionCon=0.5, modelComplexity=1, trackCon=0.5):
        """
        Inicializa um objeto de rastreamento de mãos.

        Parâmetros:
        - mode: Modo de detecção das mãos (padrão: False)
        - maxHands: Número máximo de mãos a serem detectadas (padrão: 2)
        - detectionCon: Limite de confiança para detecção (padrão: 0.5)
        - modelComplexity: Complexidade do modelo de detecção (padrão: 1)
        - trackCon: Limite de confiança para rastreamento (padrão: 0.5)
        """
        self.mode = mode
        self.maxHands = maxHands
        self.detectionCon = detectionCon
        self.modelComplex = modelComplexity
        self.trackCon = trackCon
        self.mpHands = mp.solutions.hands
        self.hands = self.mpHands.Hands(self.mode, self.maxHands, self.modelComplex,
                                        self.detectionCon, self.trackCon)
        self.mpDraw = mp.solutions.drawing_utils

    def handsFinder(self, image, draw=True):
        """
        Encontra e desenha landmarks das mãos em uma imagem.

        Parâmetros:
        - image: A imagem de entrada
        - draw: Indica se os landmarks devem ser desenhados na imagem (padrão: True)
        
        Retorna:
        - A imagem com landmarks desenhados, se draw=True
        """
        imageRGB = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
        self.results = self.hands.process(imageRGB)

        if self.results.multi_hand_landmarks:
            for handLms in self.results.multi_hand_landmarks:
                if draw:
                    self.mpDraw.draw_landmarks(image, handLms, self.mpHands.HAND_CONNECTIONS)
        return image

    def positionFinder(self, image, handNo=0, draw=True):
        """
        Localiza e retorna as posições dos landmarks da mão especificada.

        Parâmetros:
        - image: A imagem de entrada
        - handNo: O número da mão a ser rastreada (padrão: 0)
        - draw: Indica se os landmarks devem ser desenhados na imagem (padrão: True)
        
        Retorna:
        - Uma lista de posições dos landmarks da mão especificada
        """
        lmlist = []
        if self.results.multi_hand_landmarks:
            Hand = self.results.multi_hand_landmarks[handNo]
            for id, lm in enumerate(Hand.landmark):
                h, w, c = image.shape
                cx, cy = int(lm.x * w), int(lm.y * h)
                lmlist.append([id, cx, cy])
                if draw:
                    cv2.circle(image, (cx, cy), 15, (255, 0, 255), cv2.FILLED)
        return lmlist

def main():
    cap = cv2.VideoCapture(0)
    tracker = handTracker()

    while True:
        success, image = cap.read()
        image = tracker.handsFinder(image)
        lmList = tracker.positionFinder(image)
        if len(lmList) != 0:
            print(lmList[4])

        cv2.imshow("Video", image)
        cv2.waitKey(1)

if __name__ == "__main__":
    main()
```

Este código define uma classe `handTracker` que encapsula as funcionalidades de detecção e rastreamento de mãos. Ele oferece métodos para encontrar mãos em uma imagem e localizar as posições dos landmarks (pontos-chave) das mãos detectadas. O método `main()` cria uma instância dessa classe e realiza a detecção de mãos em um fluxo de vídeo da câmera. Certifique-se de manter a indentação correta ao copiar o código para um arquivo `.py`.





