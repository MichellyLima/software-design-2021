###### UNIVERSIDADE FEDERAL DE GOIÁS </br> DESIGN DE SOFTWARE </br> MICHELLY SILVA LIMA - 20180780
### Desafio 4x4
<b>Problema:</b>
> Identificar todas as possíveis soluções do "quadrado mágico".

<img src="https://github.com/kyriosdata/desafio4x4/blob/main/imagens/4x4.png" width="300">

<b>Proposta de solução (Design): </b> <br/> <br/>
Para que se tenha uma possível solução, é necessário preencher as linhas e colunas da matriz respeitando a regra de 2 pares e 2 ímpares, totalizando a soma 34 e excluindo os números utilizados (de 1 a 16).

Uma possível solução para isso seria, considerando a matriz 4x4:

[0,0] [0,1] [0,2] [0,3] <br/>
[1,0] [1,1] [1,2] [1,3] <br/>
[2,0] [2,1] [2,2] [2,3] <br/>
[3,0] [3,1] [3,2] [3,3] <br/>

<b>1.</b> Preencher o quadrado inteiro com os números de 1 a 16, começando em [0,0] e terminando em [3,3]:
[01] [02] [03] [04] <br/>
[05] [06] [07] [08] <br/>
[09] [10] [11] [12] <br/>
[13] [14] [15] [16] <br/>



<b>2.</b> Invertemos as duas diagonais principais. 
Dessa forma, mantemos a paridade dos números (sempre dois pares e dois ímpares em linhas, colunas e diagonais) além de combinarmos valores grandes com valores menores. <br/>
[16] [02] [03] [13] <br/>
[05] [11] [10] [08] <br/>
[09] [07] [06] [12] <br/>
[04] [14] [15] [01] <br/>


<b>3.</b> Ao realizar a inversão das diagonais, teremos a soma de 34 nas linhas, colunas e diagonais.

