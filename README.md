# ProjetoFinalALA

# Chroma key
é uma técnica de efeito visual que consiste em colocar uma imagem sobre uma outra por meio do anulamento de uma cor padrão, como por exemplo o verde; é uma técnica de processamento de imagens cujo objetivo é eliminar o fundo de uma imagem para isolar os personagens ou objetos de interesse que posteriormente são combinados com uma outra imagem de fundo e é utilizado em vídeos em que se deseja substituir o fundo por algum outro vídeo ou foto.

# Imagem Digital 
composta por uma matriz, onde cada elemento da matriz em uma imagem é denominado pixel, que por sua vez, descreve, de alguma forma apropriada, uma cor para aquele ponto da imagem. Por serem pequenos e próximos uns dos outros, dificilmente são percebidos a olho nu. Assim, quanto maior o número de pixels, ou seja, quanto maior a matriz que forma a imagem, melhor é a sua resolução espacial, o que permite uma melhor diferenciação espacial entre as estruturas, ou seja mais nítidas são as imagens produzidas. O formato do pixel mais comum usado em imagens é o RGB (Red (vermelho), Green (verde) e Blue (azul)). A combinação destas cores deve possibilitar a geração quaisquer outras cores. 
 
# Vídeos
O vídeo digital é composto por uma série de imagens exibidas em rápida sucessão constante com frequências comuns de 15, 24, 30 e 60 quadros(frames) por segundo (FPS); quanto mais quadros tiver, mais detalhes do movimento serão capturados ou exibidos. Cada imagem ou quadro inclui uma varredura de pixels com largura e altura expressas em número de pixels, conhecido como resolução. Quanto maior a resolução do vídeo capturado, maior sua clareza e qualidade. 
Além disso, um vídeo pode ser uma animação, criada por imagens em sequência tendo uma ideia de movimento das cenas, uma gravação feita por imagens.

# Código Chroma key para Imagens (feito em Julia)
Em primeiro momento, carregamos a imagem ou vídeo, que possui o fundo ou o elemento verde a ser removido ou transformado, para dentro do programa. 



Assim, seguimos para a etapa de identificação dos pixels verdes, através do espaço de cor HSV, cuja primeira coordenada representa a cor, a segunda a intensidade e a terceira o brilho na função máscara verde, estabelece-se uma faixa de tons verdes segura no sentido de não estar próxima das demais cores, diminuindo a chance do programa transformar espaços da imagem não desejados.  Dessa forma, convertemos inicialmente a imagem para o formato HSV e separamos os canais de cores que desejamos manipular, ou seja o tom da cor, a saturação e o brilho. Com isso, criamos uma matriz máscara de mesmo dimensão da matriz que representa a imagem trabalhada composta unicamente por 1. Então, verificamos cada posição dentro da matriz da imagem, se o valor estiver dentro da faixa de cor estabelecida, substituímos o 1 por 0 na posição. Desse modo, teremos ao final uma máscara binária, onde os zeros representam o espaço da imagem que não será transformado, não fará parte do chroma key. 

Destarte, realizamos o produto entre a máscara binária produzida e a própria imagem, sendo assim nas posições da matriz da máscara, onde temos o valor 1, ao multiplicarmos pela imagem, passarão a ter o mesmo valor da matriz que representa a imagem naquela posição e onde temos o valor 0, haverá a perda de informação da imagem, ou seja permanecerá 0. 



Por conseguinte, carregamos a imagem, denominada plano de fundo, que irá substituir os espaços verdes da imagem trabalhada e convertemos suas dimensões, a fim de estipular um padrão de tamanho de imagens trabalhadas, como exige a linguagem Julia. Em seguida, criamos uma matriz, chamada máscara invertida, inicialmente inicializada apenas por 1 e com as mesmas dimensões da máscara binária previamente produzida, e subtraímos pela própria máscara binária, gerando uma matriz inversa a máscara binária no sentido de onde uma contém 1 a outra contém 0 e vice-versa. Por fim, realizamos o produto entre a máscara inversa produzida e o plano de fundo, sendo assim nas posições da matriz da máscara inversa, onde temos o valor 1, ao multiplicarmos pelo plano de fundo, passarão a ter o mesmo valor da matriz que representa o plano de fundo naquela posição e onde temos o valor 0, haverá a perda de informação do plano de fundo, ou seja permanecerá 0. 

Finalmente, somamos os produtos entre a máscara binária produzida e a própria imagem e entre a máscara inversa e o plano de fundo, fazendo com que onde na matriz que representa a imagem trabalhada sem os espaços verdes estava armazenado o valor 0, obtenha o valor do pixel do plano de fundo na posição análoga, e vice-versa. Portanto, juntando as duas imagens, de forma que o plano de fundo substitua os espaços da imagem que originalmente eram verdes.

# Código de pré-processamento de imagem (feito em python)
A falta de nitidez de uma imagem faz com que as divisas entres duas cores não sejam tão demarcadas, fazendo com que os pixels da fronteira tenham uma cor intermediária entre as duas, “se misturem”. Dessa forma, o programa acaba tendo dificuldade em determinar se os pixels da fronteira são verdes ou não, podendo eliminar partes da imagem durante a criação da máscara binária, que deveriam ser aproveitadas ou até mesmo incluir espaços da imagem que deveriam ser transformadas pelo chroma key. Por isso que para a utilização do chroma key, a etapa de processamento da imagem trabalhada é igualmente relevante. 

Assim, um simples código de pré-processamento de imagem, capaz de alterar o constraste, a saturação e o brilho da imagem. ja contribuiria para uma melhora consideravel da nitidez da imagem. Em primeiro momento, carregamos a imegem a ser editada para dentro do programa e atravéz da biblioteca ImageEnhance, modficamos cada um dos canais da imagem e salvamos a imagem editada.

Exemplo prático:
Temos uma imagem qualquer de um telefone com fundo verde, a mesma imagem modificada pelo código de processamento de imagem mencionado e novamente a mesma imagem transformada por um processamente de imagem profissional(https://letsenhance.io/boost) e a criação de suas respectivas mascaras binárias:

![telefonemascara](https://user-images.githubusercontent.com/109240286/179090507-5edf3458-26b8-4502-bdc7-c98e24e0be25.png)
![telefoneeditadomascara](https://user-images.githubusercontent.com/109240286/179124273-0d250dc8-df78-4582-a26f-7c79a0485c18.png)
![telefoneprocessadomascara](https://user-images.githubusercontent.com/109240286/179124287-5c131de5-ee46-432f-9d2b-c5ddf81bcfb5.png)

A imagem editada pelo código de pré-processamento, apesar de simples já obteve melhores resultados nítidos na geração da mascara binária, contendo ainda alguma presença de pixels verdes não transformados nas fronteiras, mas certamente em menor quantidade quando comparada a imagem inalterada. Enquanto a imagem processada pelo editor online não possui sequer resquicios do fundo verde original da imagem.

Assim, escolhendo um plano de fundo qualquer, temos a criação das respectivas mascaras invertidas:

![telefoneeditadoinvertido](https://user-images.githubusercontent.com/109240286/179125628-9e415f5d-c02e-452d-b34b-be231dda34b1.png)
![telefoneinvertido](https://user-images.githubusercontent.com/109240286/179125631-030efff7-ad85-44cd-aa99-c02c12ab1639.png)
![fundotelefone](https://user-images.githubusercontent.com/109240286/179125633-449465a4-4435-41b1-bbcf-8a7329185f73.png)

Aqui, o impacto da diferença da qualidade das imagens torna-se clara.

Por fim, temos as imagens transformadas pelo chroma key:


