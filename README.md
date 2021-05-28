# Contador-de-Silabas-Metricas
Notebook com um modelo que realiza divisão silábica e classifcação tónica com código adicional para escandir versos.

Aceitam-se contribuições (pull requests)


## PERGUNTAS:

#### 1) Porque é que usaste uma LSTM bidireccional como encoder em vez de uma LSTM unidireccional ?

R: É conhecido na literatura que para a esmagadora maioria dos problemas seq2seq utilizar uma LSTM bidireccional como encoder em vez de uma unidireccional gera resultados superiores. Mas também faz sentido para a task de separação da palavra em sílabas, por exemplo:

bar - bar$

batata - ba|ta$ta

Suponhamos que o encoder em ambos os casos já emitiu um "b" e um "a". Agora no caso do "bar" ele iria emitir um r, no caso da "batata" iria emitir um "|". A informação necessária para fazer a escolha certa em cada um dos casos encontra-se à direita, ainda não foi vista, daí fazer sentido utilizar um encoder constítuido por duas LSTMs: 

- Uma para contextualizar a palavra da esquerda para a direta.

- Outra para contextualizar a palavra da direita para a esquerda, ou seja  a informação que o decoder vai receber do encoder não é apenas 1 hidden state e 1 cell state final mas sim 2 hidden states e 2 cell states que vão ser concatenados, constituindo assim a representação contextualizada da sequência de carateres, a palavra.

O decoder recebe esse hidden state e cell state do encoder e cada célula LSTM vai fazer output de um símbolo e passar à célula seguinte a representação vetorial do símbolo gerado (embedding) e um novo cell state e hidden state atualizado.

#### 2) As células das LSTMs partilham parâmetros ?

R: Sim, o facto das células LSTM partilharem parâmetros é o que lhes permite generalizar para sequências de tamanhos diferentes, (para palavras de curta e maior duração), mas de notar que estes parâmetros apenas são partilhados dentro da mesma LSTM. 

Neste código ao todo são utilizadas 3 LSTM´s, duas como encoder que constituem a LSTM bidireccional e uma como decoder que constitui a LSTM unidirecional. Os parâmetros destas 3 LSTM´s são diferentes, (por exemplo: os padrões para aprender as regras que codificam uma palavra de frente para trás são diferente dos parâmetros que a codificam de trás para a frente) apenas interiormente as células de cada uma são iguais.



## Demonstração:
https://www.youtube.com/watch?v=zroU2diFogk&t=15s
