# ProjetoFinalALA

# Chroma key
é uma técnica de efeito visual que consiste em colocar uma imagem sobre uma outra por meio do anulamento de uma cor padrão, como por exemplo o verde; é uma técnica de processamento de imagens cujo objetivo é eliminar o fundo de uma imagem para isolar os personagens ou objetos de interesse que posteriormente são combinados com uma outra imagem de fundo e é utilizado em vídeos em que se deseja substituir o fundo por algum outro vídeo ou foto.

# Imagem Digital 
composta por uma matriz, onde cada elemento da matriz em uma imagem é denominado pixel, que por sua vez, descreve, de alguma forma apropriada, uma cor para aquele ponto da imagem. Por serem pequenos e próximos uns dos outros, dificilmente são percebidos a olho nu. Assim, quanto maior o número de pixels, ou seja, quanto maior a matriz que forma a imagem, melhor é a sua resolução espacial, o que permite uma melhor diferenciação espacial entre as estruturas, ou seja mais nítidas são as imagens produzidas. O formato do pixel mais comum usado em imagens é o RGB (Red (vermelho), Green (verde) e Blue (azul)). A combinação destas cores deve possibilitar a geração quaisquer outras cores. 
 
# Código Chroma key para Imagens (feito em Julia)
Em primeiro momento, carregamos a imagem, que possui o fundo ou o elemento verde a ser removido ou transformado, para dentro do programa. 

Assim, seguimos para a etapa de identificação dos pixels verdes, através do espaço de cor HSV, cuja primeira coordenada representa a cor, a segunda a intensidade e a terceira o brilho na função máscara verde, estabelece-se uma faixa de tons verdes segura no sentido de não estar próxima das demais cores, diminuindo a chance do programa transformar espaços da imagem não desejados.  Dessa forma, convertemos inicialmente a imagem para o formato HSV e separamos os canais de cores que desejamos manipular, ou seja o tom da cor, a saturação e o brilho. Com isso, criamos uma matriz máscara de mesmo dimensão da matriz que representa a imagem trabalhada composta unicamente por 1. Então, verificamos cada posição dentro da matriz da imagem, se o valor estiver dentro da faixa de cor estabelecida, substituímos o 1 por 0 na posição. Desse modo, teremos ao final uma máscara binária, onde os zeros representam o espaço da imagem que não será transformado, não fará parte do chroma key. 

Destarte, realizamos o produto entre a máscara binária produzida e a própria imagem, sendo assim nas posições da matriz da máscara, onde temos o valor 1, ao multiplicarmos pela imagem, passarão a ter o mesmo valor da matriz que representa a imagem naquela posição e onde temos o valor 0, haverá a perda de informação da imagem, ou seja permanecerá 0. 

![pisamascara](https://user-images.githubusercontent.com/109240286/179271389-bdf68076-5042-4b5f-b518-1ed06490e78e.png)

Por conseguinte, carregamos a imagem, denominada plano de fundo, que irá substituir os espaços verdes da imagem trabalhada e convertemos suas dimensões, a fim de estipular um padrão de tamanho de imagens trabalhadas, como exige a linguagem Julia. Em seguida, criamos uma matriz, chamada máscara invertida, inicialmente inicializada apenas por 1 e com as mesmas dimensões da máscara binária previamente produzida, e subtraímos pela própria máscara binária, gerando uma matriz inversa a máscara binária no sentido de onde uma contém 1 a outra contém 0 e vice-versa. Por fim, realizamos o produto entre a máscara inversa produzida e o plano de fundo, sendo assim nas posições da matriz da máscara inversa, onde temos o valor 1, ao multiplicarmos pelo plano de fundo, passarão a ter o mesmo valor da matriz que representa o plano de fundo naquela posição e onde temos o valor 0, haverá a perda de informação do plano de fundo, ou seja permanecerá 0. 

![pisainverida](https://user-images.githubusercontent.com/109240286/179271386-8d638c6f-d5c1-4731-9932-1fe9a8d188ef.png)

Finalmente, somamos os produtos entre a máscara binária produzida e a própria imagem e entre a máscara inversa e o plano de fundo, fazendo com que onde na matriz que representa a imagem trabalhada sem os espaços verdes estava armazenado o valor 0, obtenha o valor do pixel do plano de fundo na posição análoga, e vice-versa. Portanto, juntando as duas imagens, de forma que o plano de fundo substitua os espaços da imagem que originalmente eram verdes.

![pisachroma](https://user-images.githubusercontent.com/109240286/179271376-f93ac278-e63b-42d3-a156-23944115c6ac.png)

# Código de pré-processamento de imagem (feito em python)
A falta de nitidez de uma imagem faz com que as divisas entres duas cores não sejam tão demarcadas, fazendo com que os pixels da fronteira tenham uma cor intermediária entre as duas, “se misturem”. Dessa forma, o programa acaba tendo dificuldade em determinar se os pixels da fronteira são verdes ou não, podendo eliminar partes da imagem durante a criação da máscara binária, que deveriam ser aproveitadas ou até mesmo incluir espaços da imagem que deveriam ser transformadas pelo chroma key. Por isso que para a utilização do chroma key, a etapa de processamento da imagem trabalhada é igualmente relevante. 

Assim, um simples código de pré-processamento de imagem, capaz de alterar o constraste, a saturação e o brilho da imagem. ja contribuiria para uma melhora consideravel da nitidez da imagem. Em primeiro momento, carregamos a imegem a ser editada para dentro do programa e atravéz da biblioteca ImageEnhance, modficamos cada um dos canais da imagem e salvamos a imagem editada.

Exemplo prático:
Temos uma imagem qualquer de um telefone com fundo verde, a mesma imagem modificada pelo código de processamento de imagem mencionado e novamente a mesma imagem transformada por um processamente de imagem profissional(https://letsenhance.io/boost) e a criação de suas respectivas mascaras binárias pelo código do chroma key:

![telefonemascara](https://user-images.githubusercontent.com/109240286/179090507-5edf3458-26b8-4502-bdc7-c98e24e0be25.png)
![telefoneeditadomascara](https://user-images.githubusercontent.com/109240286/179124273-0d250dc8-df78-4582-a26f-7c79a0485c18.png)
![telefoneprocessadomascara](https://user-images.githubusercontent.com/109240286/179124287-5c131de5-ee46-432f-9d2b-c5ddf81bcfb5.png)

A imagem editada pelo código de pré-processamento, apesar de simples já obteve melhores resultados nítidos na geração da mascara binária, contendo ainda alguma presença de pixels verdes não transformados nas fronteiras, mas certamente em menor quantidade quando comparada a imagem inalterada. Enquanto a imagem processada pelo editor online não possui sequer resquicios do fundo verde original da imagem.

Assim, escolhendo um plano de fundo qualquer, temos a criação das respectivas mascaras invertidas:

![telefoneinvertido](https://user-images.githubusercontent.com/109240286/179125631-030efff7-ad85-44cd-aa99-c02c12ab1639.png)
![telefoneeditadoinvertido](https://user-images.githubusercontent.com/109240286/179126112-75c8256f-bb8e-401e-9e80-d3650770407c.png)
![telefoneeprocessadoinvertido](https://user-images.githubusercontent.com/109240286/179125628-9e415f5d-c02e-452d-b34b-be231dda34b1.png)

Aqui, o impacto da diferença da qualidade das imagens torna-se clara.

Por fim, temos as imagens transformadas pelo chroma key:

![telefonetransformado](https://user-images.githubusercontent.com/109240286/179125961-7c74a66e-d7ea-4b97-b59e-261e463fe6a6.png)
![telefoneeditadotrsnformado](https://user-images.githubusercontent.com/109240286/179125958-abeaae33-b77d-4f01-b420-536b78213d7e.png)
<div><img src="https://user-images.githubusercontent.com/109240286/179125944-35c6eb48-4908-48c4-82e2-4c3cb46909ec.png" width="615px" /></div>


# Vídeos
O vídeo digital é composto por uma série de imagens exibidas em rápida sucessão constante com frequências comuns de 15, 24, 30 e 60 quadros(frames) por segundo (FPS); quanto mais quadros tiver, mais detalhes do movimento serão capturados ou exibidos. Cada imagem ou quadro inclui uma varredura de pixels com largura e altura expressas em número de pixels, conhecido como resolução. Quanto maior a resolução do vídeo capturado, maior sua clareza e qualidade. 
Além disso, um vídeo pode ser uma animação, criada por imagens em sequência tendo uma ideia de movimento das cenas, uma gravação feita por imagens.

# Código de Chroma key para video
Para este código utilizaremos as mesmas funções do código de chroma key para imagens. Em primeiro momento, carregamos o video para dentro do nosso programa e definimos um tamanho padrão para as imagens que serão trabalhadas dentro do código como exige a linguegem julia. Em seguida, carregamos nosso plano de fundo para dentro do programa já convertendo suas dimensões para a padrão e transformando-o em uma matriz de pixels através do espaço de cor RGB. 

Com isso, quebramos o video em seus vários quadros(frames), e para imagem que compôem o video, a convertemos para o tamanho padrão estabelecido e a trasnformamos através da função changebg do código de chroma key para imagens. 

Por fim, para sermos capazes de criar uma animação utilizando as imagens transformadas, criamos vários objetos do tipo Plots a partir das imagens transformadas e atravéz dessa biblioteca juntamos estes objetos em um gif de 30 frames por segundo. 

Exemplo:

![paulosemcabeca](https://user-images.githubusercontent.com/109240286/179131723-d6ceba99-07e5-44ab-818f-3fad4fdc46b5.gif)
![paulosemcabecagif](https://user-images.githubusercontent.com/109240286/179131457-87932cd4-f4ba-4e1c-8d6c-a2c21cf792d1.gif)


# Os efeitos de diferentes tipos de entrada
- Saco de lixo verde

![lava](https://user-images.githubusercontent.com/109240286/179271330-2ac68786-6b80-4445-a12d-cdb9ee0ca77a.gif)

Nos experimentos do código aplicado a videos contendo os sacos de lixo, é perceptível como a luz refletida pelos sacos nos objetos e superficies a sua volta faz com que o programa identifique-os como espaços verdes a serem transfomados. No caso do video com a lava, apesar dos sacos estarem somente no chão, o programa transformou pedaços da parede e dos movéis em lava também. 

![hp](https://user-images.githubusercontent.com/109240286/179272927-94e4eb09-3be9-43d2-a43d-fc72f5ce7fb5.gif)

Da mesma forma, o próprio saco, por não ser uma superficie plana, possui pontos de sombra, que não são identificados pelo programa como verde por serem mais p´roximos do preto. No caso do video acima, o saco não foi transformado por inteiro em alguns momentos do video.

- Parede verde
- 
![bts](https://user-images.githubusercontent.com/109240286/179273957-7899fa7b-cca2-41a5-84f5-94427d2c4f7b.gif)

Em superficies lisas a execução do programa torna-se visivelmente mais limpa. 

- Fundo verde digital



