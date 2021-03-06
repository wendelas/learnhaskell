Data.List
=========

O módulo [code]Data.List[/code] obviamente lida com listas, provendo funções muito úteis 
que inclusive já conhecemos algumas (como [code]map[/code] e [code]filter[/code]) por 
[code]Prelude[/code] já importar algo por conveniência. Você não precisará importar 
[code]Data.List[/code] via importação qualificada porque não conflita com nada do [code]Prelude[/code] 
(exceto os já roubados automaticamente). Vamos dar uma olhada em algumas que ainda não conhecemos.

[function]intersperse[/function] recebe um elemento e uma lista e o repete entre cada um dos 
elementos da lista. Veja só:

[function]intercalate[/function] pega uma lista de listas e uma lista simples e a insere entre 
cada uma, retornando apenas uma lista com o resultado.

[function]transpose[/function] transpõe uma lista de listas. Vendo uma lista de listas como uma 
matriz, as colunas viram linhas e vice-versa.

Supondo que queremos somar os polinômios <i>3x<sup>2</sup> + 5x + 9</i>, <i>10x<sup>3</sup> + 9</i> e 
<i>8x<sup>3</sup> + 5x<sup>2</sup> + x - 1</i>. Podemos representá-los por meio das listas 
[code][0,3,5,9][/code], [code][10,0,0,9][/code] e [code][8,5,1,-1][/code]. Agora para somar, 
precisamos fazer só o seguinte:


Quando transpomos essas três listas, as potências de 3 estão na primeira linha, as de 2 na segunda e 
assim por diante. Mapear [code]sum[/code] desses valores produz o resultado esperado.

[function]foldl'[/function] e [function]foldl1'[/function] são versões mais trabalhadoras do que suas 
respectivas versões preguiçosas. Ao usar folds preguiçosas em listas grandes, você pode se deparar 
com um <a href="http://pt.wikipedia.org/wiki/Pilha_de_chamada#Estouro_de_pilha">estouro de pilha</a>. 
O culpado disso é exatamente a natureza preguiçosa das funções, que ao invés de realmente realizar a 
operação com o acumulado, somente diz que quando for pedido um resultado, ele irá executar mais 
essa outra conta. O problema é quando todo esse bando de promessas ultrapassa o limite da memória. 
Os folds severos não são tão passíveis de bugs por já realizar as operações intermediárias ao 
invés de encher a pilha de falsas promessas. Então se você tiver esse tipo de problema, tente 
usar suas versões trabalhadoras.

[function]concat[/function] concatena uma lista de listas em apenas uma lista.


Somente removerá um nível de camadas. Então se você quer simplificar 
[code][[[2,3],[3,4,5],[2]],[[2,3],[3,4]]][/code] - uma lista de listas, precisa concatenar duas vezes.


[function]concatMap[/function] é o mesmo que mapear e depois concatenar uma lista.


[function]and[/function] recebe uma lista de valores booleanos e retorna [code]True[/code] se todos 
forem [code]True[/code].


[function]or[/function] é relativamente parecido com o [code]and[/code], mas retorna [code]True[/code] 
se no mínimo um dos valores for [code]True[/code].


[function]any[/function] e [function]all[/function] verificam se um predicado é verdadeiro para no 
mínimo um ou todos os elementos da lista (respectivamente). Geralmente usamos uma delas ao invés de 
mapear e depois usar [code]and[/code] ou [code]or[/code].


[function]iterate[/function] toma uma função e um valor inicial. Aplica a função ao valor, depois 
aplica a função ao seu resultado, então ao seu resultado novamente... Retorna os resultados na forma 
de uma lista infinita.


[function]splitAt[/function] recebe um número e uma lista. Então separa a lista em <número> elementos, 
retornando uma tupla.


[function]takeWhile[/function] é uma função simples. Vai pegando os elementos da lista enquanto a 
verificação resulta satisfatoriamente. Pode ser bastante útil.


Digamos que queira saber a soma de todas as potências menores de 10000. Não podemos mapear 
[code](^3)[/code], aplicar um filtro e tentar somar já que filtros em listas infinitas nunca terminam. 
Você pode saber que os elementos são ascendentes, mas Haskell não. Por isso que podemos fazer isso:


Aplicamos [code](^3)[/code] em uma lista infinita e, logo que o elemento 10000 é encontrado, a lista 
é cortada. Então somamos facilmente.

[function]dropWhile[/function] é semelhante, com a diferença dele descartar todos os elementos com 
resultado verdadeiro. Quando [code]False[/code] é encontrado, retorna o resto da lista. Uma função 
fascinante.


Nos entregaram uma lista que representa os valores de estoque por datas. A lista é feita por tuplas 
onde o primeiro componente é o estoque, o segundo é o ano, o terceiro é o mês e o quarto é o dia. 
Queremos saber quando o estoque excederá os 1000 dólares pela primeira vez.


[function]span[/function] é algo como o [code]takeWhile[/code] mas que retorna duas listas. A primeira 
lista contém tudo que o [code]takeWhile[/code] retornaria com o mesmo teste. A segunda é a lista que 
seria descartada.


Enquanto que [code]span[/code] toma os elementos enquanto não aparecer o primeiro true como resultado 
do teste, [function]break[/function] para ao perceber que a função retornou true. Fazer 
[code]break p[/code] é o equivalente a [code]span (not . p)[/code].


Ao usar o [code]break[/code], a segunda lista terá o primeiro elemento como o que satisfaz o teste.

[function]sort[/function] simplesmente ordena uma lista. O tipo dos elementos da lista deve ser parte 
da typeclass [code]Ord[/code], já que como seus elementos não podem ser colocados em uma ordem, 
obviamente não podem ser ordenados.


[function]group[/function] recebe uma lista e agrupa em novas listas os elementos iguais.


Se ordenarmos e depois agruparmos uma lista, podemos descobrir quantas vezes cada elemento aparece.


[function]inits[/function] e [function]tails[/function] são semelhantes a [code]init[/code] e 
[code]tail[/code], com a diferença que as primeiras vão aplicando recursivamente o método até o fim da 
lista. Confira:


Vamos usar fold para implementar uma pesquisa por sublistas em listas.


Primeiro chamamos [code]tails[/code] com a lista que procuramos. Então compara-se cada "cauda" com a 
lista.

Com isso, criamos algo com o funcionamento da função [function]isInfixOf[/function]. 
[function]isInfixOf[/function] procura por uma sublista em uma lista e retorna [code]True[/code] se for 
encontrada em algum lugar.


[function]isPrefixOf[/function] e [function]isSuffixOf[/function] procuram por sublistas no fim e 
início de uma lista.


[function]elem[/function] e [function]notElem[/function] verificam se um elemento está ou não dentro 
de uma lista.

[function]partition[/function] recebe uma lista e um predicado e retorna um par de listas. A primeira 
lista resultante contém todos os elementos que satisfazem o predicado, a segunda que não.


É importante saber diferenciar de [code]span[/code] e [code]break[/code].


Enquanto [code]span[/code] e [code]break[/code] terminam ao encontrar o primeiro elemento que não 
satisfaz o predicado, [code]partition[/code] passa por toda a lista e separa ao encontrar o predicado.

[function]find[/function] recebe uma lista e um predicado e retorna o primeiro elemento que satisfaz 
o predicado (dentro de um valor [code]Maybe[/code]). Nós iremos ver tipos de dados algébricos melhor 
no próximo capítulo, mas por enquanto: um valor [code]Maybe[/code] (possivelmente) pode ser 
[code]Just something[/code] (apenas alguma coisa) ou [code]Nothing[/code] (nada). Do mesmo jeito que 
uma lista pode ter nenhum ou vários elementos, um valor [code]Maybe[/code] pode ter nada ou um valor. 
Do mesmo jeito que uma lista de números inteiro é [code][Int][/code], o tipo que possivelmente tem 
um inteiro é [code]Maybe Int[/code]. Bom, testemos o [code]find[/code].



Veja o tipo de [code]find[/code]. É [code]Maybe a[/code]. Isso é muito parecido com ser 
[code][a][/code], mas o [code]Maybe[/code] pode conter um ou zero elementos, enquanto uma lista pode 
conter um, zero ou vários elementos.

Lembra-se de quando procuramos a primeira vez que o estoque ultrapassou $1000. Fizemos 
[code]head (dropWhile (\(val,y,m,d) -&gt; val &lt; 1000) stock)[/code]. Lembre-se que [code]head[/code] 
não é muito seguro. O que acontece se nosso estoque nunca chegou a $1000? Nosso uso da 
[code]dropWhile[/code] retornaria uma lista vazia e ao pegarmos o primeiro elemento de nada seria 
gerado um erro. No entanto, se reescrevêssemos para [code]find (\(val,y,m,d) -&gt; val &gt; 1000) 
stock[/code], seria muito mais seguro. Se o estoque nunca chegou a $1000 (que nenhum elemento satisfez 
	o predicado), seria retornado [code]Nothing[/code]. Mas uma resposta válida dessa lista seria,
	 digamos, [code]Just (1001.4,2008,9,4)[/code].

[function]elemIndex[/function] é semelhante ao [code]elem[/code], mas que não retorna um valor 
booleano. Provavelmente retornará a posição do elemento. Caso o elemento não exista na lista, 
[code]Nothing[/code].


[function]elemIndices[/function] é semelhante ao [code]elemIndex[/code], mas retorna uma lista de 
posições, no caso do nosso elemento aparecer várias vezes. Como estamos usando uma lista para 
representar os índices, não precisamos de um tipo [code]Maybe[/code], já que o erro pode ser 
representado por uma lista vazia, muito semelhante ao [code]Nothing[/code].


[function]findIndex[/function] é parecido com o find, mas retorna apenas a posição do primeiro 
elemento que satisfaz o predicado. [function]findIndices[/function] retorna os índices de todos os 
elementos que satisfaz o predicado na forma de uma lista. 


Já vimos [code]zip[/code] e [code]zipWith[/code]. Elas combinam ("transpõem") duas listas em tuplas 
ou por uma função binária (o que significa precisar de dois parâmetros). Mas e se quisermos combinar 
três listas? Ou combinar três listas com uma função de três parâmetros? Para isso temos as 
[function]zip3[/function], [function]zip4[/function], etc. e [function]zipWith3[/function], 
[function]zipWith4[/function], etc. Variações que vão até 7. Pode parecer uma gambiarra, mas funciona 
muito bem, além de não ser sempre que é preciso transpor 8 listas. Ainda existe um modo muito 
prático de combinar listas infinitas, mas não avançamos o suficiente para ver essa parte.


Igualmente à transposição comum, listas maiores que a menor são cortadas nessa quantidade de elementos.

[function]lines[/function] é uma função muito útil ao lidar com arquivos ou qualquer outro tipo de 
entrada. Recebe uma string e retorna cada linha da string como uma lista.


[code]'\n'[/code] é o caractere para uma nova linha Unix. Barras invertidas sempre têm um significado 
especial em strings e caracteres em Haskell.

[function]unlines[/function] é o inverso da função [code]lines[/code]. Recebe uma lista de strings 
e as une pelo caractere [code]'\n'[/code].



[function]words[/function] e [function]unwords[/function] são para separar uma linha de texto 
em palavras ou unir na ordem contrária. Muito útil.


Já mencionamos a função [function]nub[/function]. Recebe uma lista e retira os elementos duplicados, 
retornando uma lista que temos a certeza dos elementos não possuírem nenhum gêmeo perdido. Mas essa 
função tem um nome bem curioso. A expressão "nub" significa a parte essencial ou prioritária de algo. 
Na minha opinião, nomes de funções deveriam ter nomes mais parecidos com o seu real significado ao 
invés de se prender a antigas definições.


[function]delete[/function] recebe um elemento e uma lista e remove sua primeira ocorrência.


[function]\\[/function] é uma função de diferença de listas. Resumindo, traz a diferença. Para cada 
elemento da lista 2, remove-se o correspondente (se existente) na 1.


Fazer [code][1..10] \\ [2,5,9][/code] é o mesmo que [code]delete 2 . delete 5 . delete 9 $ [1..10][/code].

[function]union[/function] também faz uma operação de conjuntos. Retorna a união de duas listas. 
Funciona colocando cada elemento da lista 2 na 1, se não existente.


[function]intersect[/function] significa intersecção. Retorna apenas os elementos encontrados em ambas 
listas.


[function]insert[/function] recebe um elemento e uma lista de elementos que podem ser ordenados. 
Então, o insere logo depois que o menor ou igual mais próximo. Se usarmos [code]insert[/code] em uma 
lista ordenada, o resultado permanecerá ordenado.



[code]length[/code], [code]take[/code], [code]drop[/code], [code]splitAt[/code], [code]!![/code] e 
[code]replicate[/code] têm em comum receberem um [code]Int[/code] como parâmetro, mesmo sabendo que o 
código ficaria mais genérico aceitando todos tipos de [code]Integral[/code] ou [code]Num[/code] 
(dependendo de cada função). Isso é feito por razões de compatibilidade. Mudar isso com certeza 
quebraria muito código antigo. Por isso que [code]Data.List[/code] implementou versões suas mais 
genéricas: [function]genericLength[/function], [function]genericTake[/function], 
[function]genericDrop[/function], [function]genericSplitAt[/function], [function]genericIndex[/function] 
e [function]genericReplicate[/function]. [code]length[/code] tem tipo [code]length :: [a] -&gt; Int[/code]. 
Se você tentar descobrir a média de números com [code]let xs = [1..6] in sum xs / length xs[/code] 
terá um erro, já que você não pode usar [code]/[/code] com [code]Int[/code]. [code]genericLength[/code] 
por outro lado, tem tipo [code]genericLength :: (Num a) =&gt; [b] -&gt; a[/code]. Como 
[code]Num[/code] pode agir como ponto flutuante, você pode conseguir o resultado esperado com 
[code]let xs = [1..6] in sum xs / genericLength xs[/code].

[code]nub[/code], [code]delete[/code], [code]union[/code], [code]intersect[/code] e 
[code]group[/code] têm também versões genéricas com nomes [function]nubBy[/function], 
[function]deleteBy[/function], [function]unionBy[/function], [function]intersectBy[/function] e 
[function]groupBy[/function]. A diferença é que as primeiras versões usam [code]==[/code] para testar 
por igualdade, enquanto suas versões <i>By</i> requerem também uma função para comparação. 
[code]group[/code] é o mesmo que [code]groupBy (==)[/code].

Por hora, digamos que temos uma lista que descreve os valores de uma segunda e nós queremos 
dividir em sublistas baseado se o elemento está abaixo ou acima de zero. Se usássemos o 
[code]group[/code] comum, apenas agruparia valores adjacentes. Mas queremos classificar quanto à 
posição de zero. É aí que [code]groupBy[/code] mostra sua utilidade! A função de igualdade das 
funções <i>By</i> precisa receber dois elementos de mesmo tipo e retornar [code]True[/code], 
independente dos seus critérios para isso.


Logo identificamos como fazer nosso teste. A função de igualdade retornará [code]True[/code] 
somente se ambos forem negativos ou positivos. Essa função de igualdade pode ser escrita por 
[code]\x y -&gt; (x &gt; 0) &amp;&amp; (y &gt; 0) || (x &lt;= 0) &amp;&amp; (y &lt;= 0)[/code], 
apesar de achar que do primeiro jeito ficou muito mais legível. Um modo muito melhor de escrever 
funções <i>By</i> é importando a função [function]on[/function] de [code]Data.Function[/code]. 
[code]on[/code] é definida assim:

Fazendo [code](==) `on` (&gt; 0)[/code] retorna uma função de igualdade semelhante a 
[code]\x y -&gt; (x &gt; 0) == (y &gt; 0)[/code]. [code]on[/code] é muito usada por funções <i>By</i>, 
já que é possível fazer isso:


Muito mais legível! Agora você pode ler em voz alta: Agrupe por igualdade sempre que elementos 
forem maiores do que zero.

Semelhantemente, [code]sort[/code], [code]insert[/code], [code]maximum[/code] e [code]minimum[/code] 
também têm versões genéricas. Funções como [code]groupBy[/code] recebem uma função para determinar 
quando dois elementos são iguais. [function]sortBy[/function], [function]insertBy[/function], 
[function]maximumBy[/function] e [function]minimumBy[/function] recebem uma função que diz se um 
elemento é maior, menor ou igual a outro. O tipo de [code]sortBy[/code] e 
[code]sortBy :: (a -&gt; a -&gt; Ordering) -&gt; [a] -&gt; [a][/code]. Se você ainda se lembra, o 
tipo [code]Ordering[/code] pode assumir valor [code]LT[/code], [code]EQ[/code] ou [code]GT[/code]. 
[code]sort[/code] é o equivalente a [code]sortBy compare[/code], porque <i>compare</i> pega dois tipos de 
[code]Ord[/code] e retorna sua relação quanto ordenação.

Listas podem ser comparadas, mas quando o são, são comparados lexicograficamente. E se quisermos 
ordenar uma lista de listas não baseado nas listas internas, mas em seus tamanhos? Como deve ter 
imaginado, [code]sortBy[/code] é a nossa solução.


Incrível! [code]compare `on` length[/code] ... cara, isso mais parece inglês puro! Se você não 
percebeu bem como [code]on[/code] funciona aqui, [code]compare `on` length[/code] é o equivalente a 
[code]\x y -&gt; length x `compare` length y[/code]. Quando você está trabalhando com funções 
<i>By</i> que precisam de uma função de igualdade, você geralmente vai usar 
[code](==) `on` something[/code] e com funções <i>By</i> que precisem de outra de ordenação, você 
usa [code]compare `on` something[/code].
