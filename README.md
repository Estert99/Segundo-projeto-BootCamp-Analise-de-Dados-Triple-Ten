# Segundo-projeto-BootCamp-Analise-de-Dados-Triple-Ten
Projeto de conclusão da segunda sprint do BootCamp de Analise de Dados.
# Se liga na música

# Conteúdo <a id='back'></a>

* [Introdução](#intro)
* [Etapa 1. Visão geral dos dados](#data_review)
    * [Conclusões](#data_review_conclusions)
* [Etapa 2. Pré-processamento de dados](#data_preprocessing)
    * [2.1 Estilo do cabeçalho](#header_style)
    * [2.2 Valores ausentes](#missing_values)
    * [2.3 Duplicados](#duplicates)
    * [2.4 Conclusões](#data_preprocessing_conclusions)
* [Etapa 3. Teste da hipótese](#hypothesis)
    * [3.1 Hipótese 1: atividade dos usuários nas duas cidades](#activity)
* [Conclusões](#end)

## Introdução <a id='intro'></a>
O trabalho de um analista é analisar dados para obter percepções valiosas dos dados e tomar decisões fundamentadas neles. Esse processo consiste em várias etapas, como visão geral dos dados, pré-processamento dos dados e testes de hipóteses.

Sempre que fazemos uma pesquisa, precisamos formular uma hipótese que depois poderemos testar. Às vezes nós aceitamos essas hipóteses; outras vezes, nós as rejeitamos. Para fazer as escolhas certas, um negócio deve ser capaz de entender se está fazendo as suposições certas ou não.

Neste projeto, você vai comparar as preferências musicais dos habitantes de Springfild e Shelbyville. Você vai estudar os dados de um serviço de streaming de música online para testar a hipótese apresentada abaixo e comparar o comportamento dos usuários dessas duas cidades.

### Objetivo:
Teste a hipótese:
1. A atividade dos usuários é diferente dependendo do dia da semana e da cidade.


### Etapas
Os dados sobre o comportamento do usuário são armazenados no arquivo `/datasets/music_project_en.csv`. Não há informações sobre a qualidade dos dados, então será necessário examiná-los antes de testar a hipótese.

Primeiro, você avaliará a qualidade dos dados e verá se seus problemas são significativos. Depois, durante o pré-processamento dos dados, você tentará tratar dos problemas mais críticos.

O seu projeto consistirá em três etapas:
 1. Visão geral dos dados
 2. Pré-processamento de dados
 3. Teste da hipótese








[Voltar ao Índice](#back)

## Etapa 1. Visão geral dos dados <a id='data_review'></a>

Abra os dados e examine-os.

Você precisará da `pandas`, então, importe-a.

```python
# importando panda
import pandas as pd
```

Leia o arquivo `music_project_en.csv` da pasta `/datasets/` e salve-o na variável `df`:

```python
# lendo o arquivo e armazenando em df
df=pd.read_csv('/datasets/music_project_en.csv')
```

Imprima as primeiras 10 linhas da tabela:

```python
# obtenha as 10 primeiras 10 linhas da tabela df
print(df.head(10))
```

```
Saída:
     userID                        Track            artist   genre  \
0  FFB692EC            Kamigata To Boots  The Mass Missile    rock   
1  55204538  Delayed Because of Accident  Andreas Rönnberg    rock   
2    20EC38            Funiculì funiculà       Mario Lanza     pop   
3  A3DD03C9        Dragons in the Sunset        Fire + Ice    folk   
4  E2DC1FAE                  Soul People        Space Echo   dance   
5  842029A1                       Chains          Obladaet  rusrap   
6  4CB90AA5                         True      Roman Messer   dance   
7  F03E1C1F             Feeling This Way   Polina Griffith   dance   
8  8FA1D3BE                     L’estate       Julia Dalia  ruspop   
9  E772D5C0                    Pessimist               NaN   dance   

        City        time        Day  
0  Shelbyville  20:28:33  Wednesday  
1  Springfield  14:07:09     Friday  
2  Shelbyville  20:58:07  Wednesday  
3  Shelbyville  08:37:09     Monday  
4  Springfield  08:34:34     Monday  
5  Shelbyville  13:09:41     Friday  
6  Springfield  13:00:07  Wednesday  
7  Springfield  20:47:49  Wednesday  
8  Springfield  09:17:40     Friday  
9  Shelbyville  21:20:49  Wednesday  
```

Obtenha informações gerais sobre a tabela usando um comando. Você conhece o método para exibir informações gerais que precisamos obter.

```python
# obtendo informações gerais sobre os nossos dados
print(df.info())
```

```
Saída:
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 65079 entries, 0 to 65078
Data columns (total 7 columns):
 #   Column    Non-Null Count  Dtype 
---  ------    --------------  ----- 
 0     userID  65079 non-null  object
 1   Track     63736 non-null  object
 2   artist    57512 non-null  object
 3   genre     63881 non-null  object
 4     City    65079 non-null  object
 5   time      65079 non-null  object
 6   Day       65079 non-null  object
dtypes: object(7)
memory usage: 3.5+ MB
None
```

Aqui estão as nossas observações sobre a tabela. Ela contém sete colunas. Elas armazenam o mesmo tipo de dado: `object`.

De acordo com a documentação:
- `' userID'` — identificação do usuário
- `'Track'` — título da música
- `'artist'` — nome do artista
- `'genre'` — gênero da música
- `'City'` — cidade do usuário
- `'time'` — o tempo exato que a música foi reproduzida
- `'Day'` — dia da semana

Podemos ver três problemas de estilo nos cabeçalhos da tabela:
1. Alguns cabeçalhos são escritos em letras maiúsculas, outros estão em minúsculas.
2. Alguns cabeçalhos contêm espaços.
3. A coluna 'userID' esta sem o _ 






### Escreva suas observações. Aqui estão algumas perguntas que podem ajudar: <a id='data_review_conclusions'></a>

`1.   Que tipo de dados temos nas linhas? E como podemos entender as colunas?`
R. Nós temos dados do tipo objeto nas colunas (#usei o print .info()) para obter os tipos dos dados).
Para entendermos o que cada coluna representa, utilizei o método head(), que mostra as primeiras linhas com os dados. Isso me ajudou a analisar o significado de cada coluna a partir dos valores extraídos.


`2.   Esses dados são suficientes para responder à nossa hipótese ou precisamos de mais dados?`
R. Os dados são suficientes para uma analise inicial, pois contem as colunas necessarias para(userId, track,data,city, genre).Mas com resalvas, ainda precisamos tratar as duplicadas e os valores nulos, alem disso o Data Frame nos traz somente 3 dias semana.
Seria interessante que pudéssemos analisar todos os dias da semana e, assim, ter uma ideia clara de como a atividade dos usuários se comporta no geral.

`3.   Você notou algum problema nos dados, como valores ausentes, duplicados ou tipos de dados errados`
Notei que na coluna 'artist' temos valores nulos, o cabeçalho tem problemas de estilo e formatação incorreta,  há duplicatas implícitas no gênero musical.

[Voltar ao Índice](#back)

## Etapa 2. Pré-processamento de dados <a id='data_preprocessing'></a>

O objetivo aqui é preparar os dados para a análise.
O primeiro passo é resolver todos os problemas com o cabeçalho. E então podemos passar para os valores ausentes e duplicados. Vamos começar.

Corrija a formatação nos cabeçalhos da tabela.


### Estilo do cabeçalho <a id='header_style'></a>
Imprima os cabeçalhos da tabela (os nomes das colunas):

```python
# imprima os nomes das colunas
import pandas as pd 

df=pd.read_csv('/datasets/music_project_en.csv')

df.columns = (
    df.columns
    .str.strip()                      
    .str.lower()                      
    .str.replace(' ', '_', regex=False))

print(df.columns)
```

```
Saída:
Index(['userid', 'track', 'artist', 'genre', 'city', 'time', 'day'], dtype='object')
```

Mude os cabeçalhos da tabela conforme as boas práticas de estilo:
* Todos os caracteres precisam estar com letras minúsculas
* Exclua espaços
* Se o nome tiver várias palavras, use snake_case

Anteriormente, você aprendeu sobre uma maneira automatizada de renomear colunas. Vamos usá-la agora. Use o ciclo for para percorrer os nomes das colunas e transformar todos os caracteres em letras minúsculas. Após fazer isso, imprima os cabeçalhos da tabela novamente:

```python
# Percorrendo os cabeçalhos e convertendo tudo em minúsculos
import pandas as pd

df=pd.read_csv ('/datasets/music_project_en.csv')

novas_colunas = []

for coluna in df.columns:
        coluna_striped=coluna.strip().lower().replace(' ','_')
        novas_colunas.append(coluna_striped)
df.columns = novas_colunas

print(df.columns)

     
```

```
Saída:
Index(['userid', 'track', 'artist', 'genre', 'city', 'time', 'day'], dtype='object')
```

Agora, usando a mesma abordagem, exclua os espaços no início e no final de cada nome de coluna e imprima os nomes das colunas novamente:

```python

import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')

novas_colunas = []

for coluna in df.columns:
        coluna_striped=coluna.strip().replace(' ','_')
        novas_colunas.append(coluna_striped)
df.columns = novas_colunas

print(df.columns)
```

```
Saída:
Index(['userID', 'Track', 'artist', 'genre', 'City', 'time', 'Day'], dtype='object')
```

Precisamos aplicar a regra de sublinhado no lugar de espaço à coluna `userid`. Deveria ser `user_id`. Renomeie essa coluna e imprima os nomes de todas as colunas quando terminar.

```python
# Renomeando a coluna "userid"


import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')

novas_colunas = []

for coluna in df.columns:
        coluna_striped=coluna.strip().lower().replace(' ','_')
        novas_colunas.append(coluna_striped)
df.columns = novas_colunas

df.rename(columns = {'userid': 'user_id'}, inplace=True)

print(df.columns)
```

```
Saída:
Index(['user_id', 'track', 'artist', 'genre', 'city', 'time', 'day'], dtype='object')
```

Verifique o resultado. Imprima os cabeçalhos novamente:

```python
# verificando o resultado: a lista de cabeçalhos

import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')


novas_colunas = []

for coluna in df.columns:
        coluna_striped=coluna.strip().lower().replace(' ','_')
        novas_colunas.append(coluna_striped)
df.columns = novas_colunas


df.rename(columns = {'userid': 'user_id'}, inplace=True)


print(df.columns)
```

```
Saída:
Index(['user_id', 'track', 'artist', 'genre', 'city', 'time', 'day'], dtype='object')
```

[Voltar ao Índice](#back)

### Valores Ausentes <a id='missing_values'></a>
 Primeiro, encontre a quantidade de valores ausentes na tabela. Você precisa usar dois métodos em sequência para obter o número de valores ausentes.

```python
# calculando o número de valores ausentes

import pandas as pd

df=pd.read_csv ('/datasets/music_project_en.csv')

print(df.head(10))
print(df.isna().sum())
```

```
Saída:
     userID                        Track            artist   genre  \
0  FFB692EC            Kamigata To Boots  The Mass Missile    rock   
1  55204538  Delayed Because of Accident  Andreas Rönnberg    rock   
2    20EC38            Funiculì funiculà       Mario Lanza     pop   
3  A3DD03C9        Dragons in the Sunset        Fire + Ice    folk   
4  E2DC1FAE                  Soul People        Space Echo   dance   
5  842029A1                       Chains          Obladaet  rusrap   
6  4CB90AA5                         True      Roman Messer   dance   
7  F03E1C1F             Feeling This Way   Polina Griffith   dance   
8  8FA1D3BE                     L’estate       Julia Dalia  ruspop   
9  E772D5C0                    Pessimist               NaN   dance   

        City        time        Day  
0  Shelbyville  20:28:33  Wednesday  
1  Springfield  14:07:09     Friday  
2  Shelbyville  20:58:07  Wednesday  
3  Shelbyville  08:37:09     Monday  
4  Springfield  08:34:34     Monday  
5  Shelbyville  13:09:41     Friday  
6  Springfield  13:00:07  Wednesday  
7  Springfield  20:47:49  Wednesday  
8  Springfield  09:17:40     Friday  
9  Shelbyville  21:20:49  Wednesday  
  userID       0
Track       1343
artist      7567
genre       1198
  City         0
time           0
Day            0
dtype: int64
```

Nem todos os valores ausentes afetam a pesquisa. Por exemplo, os valores ausentes em `track` e `artist` não são críticos. Você pode simplesmente substituí-los por valores padrão, como a string `'unknown'`.

Mas valores ausentes em `'genre'` podem afetar a comparação de preferências musicais de Springfield e Shelbyville. Na vida real, seria útil descobrir as razões pelas quais os dados estão ausentes e tentar corrigi-los. Mas nós não temos essa possibilidade neste projeto. Então, você terá que:
* Preencha esses valores ausentes com um valor padrão
* Avalie em que medida os valores ausentes podem afetar sua análise

Substitua os valores ausentes nas colunas `'track'`, `'artist'` e `'genre'` pela string `'unknown'`. Como mostramos nas lições anteriores, a melhor maneira de fazer isso é criar uma lista para armazenar os nomes das colunas nas quais precisamos fazer a substituição. Em seguida, use essa lista e percorra as colunas nas quais a substituição seja necessária e faça a substituição.

```python
# percorrendo os cabeçalhos e substituindo valores ausentes por 'unknown'
import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')

columns_to_replace = ['Track', 'artist', 'genre']

for coluna in columns_to_replace:
    df[coluna] = df[coluna].fillna('unknown')
    
print(df[columns_to_replace].isna().sum())
```

```
Saída:
Track     0
artist    0
genre     0
dtype: int64
```

Agora verifique o resultado para ter certeza de que o conjunto de dados não contenha valores ausentes após a substituição. Para fazer isso, conte os valores ausentes novamente.

```python
# contando os valores ausentes


import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')



novas_colunas = []

for coluna in df.columns:
        coluna_striped=coluna.strip().lower().replace(' ','_')
        novas_colunas.append(coluna_striped)
df.columns = novas_colunas


df.rename(columns = {'userid': 'user_id'}, inplace=True)


columns_to_replace = ['track', 'artist', 'genre']

for coluna in columns_to_replace:
    df[coluna] = df[coluna].fillna('unknown')
    
print(df[columns_to_replace].isna().sum())

print(df.columns)
print(df.head(10))
print(df.isna().sum())
```

```
Saída:
track     0
artist    0
genre     0
dtype: int64
Index(['user_id', 'track', 'artist', 'genre', 'city', 'time', 'day'], dtype='object')
    user_id                        track            artist   genre  \
0  FFB692EC            Kamigata To Boots  The Mass Missile    rock   
1  55204538  Delayed Because of Accident  Andreas Rönnberg    rock   
2    20EC38            Funiculì funiculà       Mario Lanza     pop   
3  A3DD03C9        Dragons in the Sunset        Fire + Ice    folk   
4  E2DC1FAE                  Soul People        Space Echo   dance   
5  842029A1                       Chains          Obladaet  rusrap   
6  4CB90AA5                         True      Roman Messer   dance   
7  F03E1C1F             Feeling This Way   Polina Griffith   dance   
8  8FA1D3BE                     L’estate       Julia Dalia  ruspop   
9  E772D5C0                    Pessimist           unknown   dance   

          city      time        day  
0  Shelbyville  20:28:33  Wednesday  
1  Springfield  14:07:09     Friday  
2  Shelbyville  20:58:07  Wednesday  
3  Shelbyville  08:37:09     Monday  
4  Springfield  08:34:34     Monday  
5  Shelbyville  13:09:41     Friday  
6  Springfield  13:00:07  Wednesday  
7  Springfield  20:47:49  Wednesday  
8  Springfield  09:17:40     Friday  
9  Shelbyville  21:20:49  Wednesday  
user_id    0
track      0
artist     0
genre      0
city       0
time       0
day        0
dtype: int64
```

[Voltar ao Índice](#back)

### Duplicados <a id='duplicates'></a>
Encontre o número de duplicados explícitos na tabela. Lembre-se de que você precisa aplicar dois métodos em sequência para obter o número de duplicados explícitos.

```python
# contando duplicados explícitos

import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')

print(df.duplicated().sum())
```

```
Saída:
3826
```

Agora descarte todos os duplicados. Para fazer isso, chame o método que faz exatamente isso.

```python
# removendo duplicados explícitos
import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')



df= df.drop_duplicates()
print(df.duplicated().sum())
print(df.head(10))
```

```
Saída:
0
     userID                        Track            artist   genre  \
0  FFB692EC            Kamigata To Boots  The Mass Missile    rock   
1  55204538  Delayed Because of Accident  Andreas Rönnberg    rock   
2    20EC38            Funiculì funiculà       Mario Lanza     pop   
3  A3DD03C9        Dragons in the Sunset        Fire + Ice    folk   
4  E2DC1FAE                  Soul People        Space Echo   dance   
5  842029A1                       Chains          Obladaet  rusrap   
6  4CB90AA5                         True      Roman Messer   dance   
7  F03E1C1F             Feeling This Way   Polina Griffith   dance   
8  8FA1D3BE                     L’estate       Julia Dalia  ruspop   
9  E772D5C0                    Pessimist               NaN   dance   

        City        time        Day  
0  Shelbyville  20:28:33  Wednesday  
1  Springfield  14:07:09     Friday  
2  Shelbyville  20:58:07  Wednesday  
3  Shelbyville  08:37:09     Monday  
4  Springfield  08:34:34     Monday  
5  Shelbyville  13:09:41     Friday  
6  Springfield  13:00:07  Wednesday  
7  Springfield  20:47:49  Wednesday  
8  Springfield  09:17:40     Friday  
9  Shelbyville  21:20:49  Wednesday  
```

Agora vamos verificar se descartamos todos os duplicados. Conte duplicados explícitos mais uma vez para ter certeza de que você removeu todos eles:

```python
# verificando duplicados novamente
import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')

df= df.drop_duplicates()

print(df.duplicated().sum())
```

```
Saída:
0
```

Agora queremos nos livrar dos duplicados implícitos na coluna `genre`. Por exemplo, o nome de um gênero pode ser escrito de maneiras diferentes. Alguns erros afetarão também o resultado.

Para fazer isso, vamos começar imprimindo uma lista de nomes de gênero únicos, ordenados em ordem alfabética: Para fazer isso:
* Extraia a coluna `genre` do DataFrame
* Chame o método que retornará todos os valores únicos na coluna extraída


```python
# visualizando nomes de gêneros únicos
import pandas as pd
df=pd.read_csv ('/datasets/music_project_en.csv')

genre_column = df['genre']
genre_column.unique()

df= df.drop_duplicates()

print(genre_column)

print(df.duplicated().sum())
```

```
Saída:
0              rock
1              rock
2               pop
3              folk
4             dance
            ...    
65074           rnb
65075           hip
65076    industrial
65077          rock
65078       country
Name: genre, Length: 65079, dtype: object
0
```

Olhe a lista e encontre duplicados implícitos do gênero `hiphop`. Esses podem ser nomes escritos incorretamente, ou nomes alternativos para o mesmo gênero.

Você verá os seguintes duplicados implícitos:
* `hip`
* `hop`
* `hip-hop`

Para se livrar deles, crie uma função `replace_wrong_genres()` com dois parâmetros:
* `wrong_genres=` — essa é uma lista que contém todos os valores que você precisa substituir
* `correct_genre=` — essa é uma string que você vai usar para a substituição

Como resultado, a função deve corrigir os nomes na coluna `'genre'` da tabela `df`, isto é, substituindo cada valor da lista `wrong_genres` por valores de `correct_genre`.

Dentro do corpo da função, use um ciclo `'for'` para percorrer a lista de gêneros errados, extrair a coluna `'genre'` e aplicar o método `replace` para fazer as correções.

```python
# função para substituir duplicados implícitos
def replace_wrong_genres(wrong_genres, correct_genre):
    for wrong_genre in wrong_genres:
        df['genre'] = df['genre'].replace(wrong_genre, correct_genre)

print(df.head(10))
```

```
Saída:
     userID                        Track            artist   genre  \
0  FFB692EC            Kamigata To Boots  The Mass Missile    rock   
1  55204538  Delayed Because of Accident  Andreas Rönnberg    rock   
2    20EC38            Funiculì funiculà       Mario Lanza     pop   
3  A3DD03C9        Dragons in the Sunset        Fire + Ice    folk   
4  E2DC1FAE                  Soul People        Space Echo   dance   
5  842029A1                       Chains          Obladaet  rusrap   
6  4CB90AA5                         True      Roman Messer   dance   
7  F03E1C1F             Feeling This Way   Polina Griffith   dance   
8  8FA1D3BE                     L’estate       Julia Dalia  ruspop   
9  E772D5C0                    Pessimist               NaN   dance   

        City        time        Day  
0  Shelbyville  20:28:33  Wednesday  
1  Springfield  14:07:09     Friday  
2  Shelbyville  20:58:07  Wednesday  
3  Shelbyville  08:37:09     Monday  
4  Springfield  08:34:34     Monday  
5  Shelbyville  13:09:41     Friday  
6  Springfield  13:00:07  Wednesday  
7  Springfield  20:47:49  Wednesday  
8  Springfield  09:17:40     Friday  
9  Shelbyville  21:20:49  Wednesday  
```

Agora, chame a função `replace_wrong_genres()` e passe argumentos apropriados para que ela limpe duplicados implícitos (`hip`, `hop` e `hip-hop`) substituindo-os por `hiphop`:

```python
# removendo duplicados implícitos
def replace_wrong_genres(wrong_genres, correct_genre):
    for wrong_genre in wrong_genres:
        df['genre'] = df['genre'].replace(wrong_genre, correct_genre)

duplicates = ['hip', 'hop', 'hip-hop']
name = 'hiphop'

print(df.head(10))
```

```
Saída:
     userID                        Track            artist   genre  \
0  FFB692EC            Kamigata To Boots  The Mass Missile    rock   
1  55204538  Delayed Because of Accident  Andreas Rönnberg    rock   
2    20EC38            Funiculì funiculà       Mario Lanza     pop   
3  A3DD03C9        Dragons in the Sunset        Fire + Ice    folk   
4  E2DC1FAE                  Soul People        Space Echo   dance   
5  842029A1                       Chains          Obladaet  rusrap   
6  4CB90AA5                         True      Roman Messer   dance   
7  F03E1C1F             Feeling This Way   Polina Griffith   dance   
8  8FA1D3BE                     L’estate       Julia Dalia  ruspop   
9  E772D5C0                    Pessimist               NaN   dance   

        City        time        Day  
0  Shelbyville  20:28:33  Wednesday  
1  Springfield  14:07:09     Friday  
2  Shelbyville  20:58:07  Wednesday  
3  Shelbyville  08:37:09     Monday  
4  Springfield  08:34:34     Monday  
5  Shelbyville  13:09:41     Friday  
6  Springfield  13:00:07  Wednesday  
7  Springfield  20:47:49  Wednesday  
8  Springfield  09:17:40     Friday  
9  Shelbyville  21:20:49  Wednesday  
```

Certifique-se que os nomes duplicados foram removidos. Imprima a lista de valores únicos da coluna `'genre'` mais uma vez:

```python
# verificando valores duplicados
def replace_wrong_genres(wrong_genres, correct_genre):
    for wrong_genre in wrong_genres:
        df['genre'] = df['genre'].replace(wrong_genre, correct_genre)

duplicates = ['hip', 'hop', 'hip-hop']
name = 'hiphop'

df['genre'].unique()

print(df.head(10))
```

```
Saída:
     userID                        Track            artist   genre  \
0  FFB692EC            Kamigata To Boots  The Mass Missile    rock   
1  55204538  Delayed Because of Accident  Andreas Rönnberg    rock   
2    20EC38            Funiculì funiculà       Mario Lanza     pop   
3  A3DD03C9        Dragons in the Sunset        Fire + Ice    folk   
4  E2DC1FAE                  Soul People        Space Echo   dance   
5  842029A1                       Chains          Obladaet  rusrap   
6  4CB90AA5                         True      Roman Messer   dance   
7  F03E1C1F             Feeling This Way   Polina Griffith   dance   
8  8FA1D3BE                     L’estate       Julia Dalia  ruspop   
9  E772D5C0                    Pessimist               NaN   dance   

        City        time        Day  
0  Shelbyville  20:28:33  Wednesday  
1  Springfield  14:07:09     Friday  
2  Shelbyville  20:58:07  Wednesday  
3  Shelbyville  08:37:09     Monday  
4  Springfield  08:34:34     Monday  
5  Shelbyville  13:09:41     Friday  
6  Springfield  13:00:07  Wednesday  
7  Springfield  20:47:49  Wednesday  
8  Springfield  09:17:40     Friday  
9  Shelbyville  21:20:49  Wednesday  
```

[Voltar ao Índice](#back)

 Suas observações <a id='data_preprocessing_conclusions'></a>

` Descreva brevemente o que você reparou ao analisar duplicados, bem como a abordagem que usou para eliminá-los e os resultados que alcançou.`
Duplicadas explicitas: linhas idênticas no DataFrame, identificadas com df.duplicated().sum(). Para eliminá-las, usei df = df.drop_duplicates().
Duplicadas implícitas: valores que representam a mesma categoria, mas escritos de formas diferentes,encontrei isso na coluna genre, onde o gênero 'hiphop' aparecia de maneiras diferentes. Para corrigir, usei a função replace_wrong_genres(), que percorre uma lista e substitui os valores por um valor correto padronizado usando o método .replace().Como resultado, temos um DataFrame com dados precisos e consistentes, sem linhas repetidas e valores desnecessários.

[Voltar ao Índice](#back)

## Etapa 3. Teste da hipótese <a id='hypothesis'></a>

### Hipótese: comparação do comportamento dos usuários nas duas cidades <a id='activity'></a>

A hipótese afirma que existem diferenças no consumo de música pelos usuários em Springfield e em Shelbyville. Para testar a hipótese, use os dados dos três dias da semana: segunda-feira (Monday), quarta-feira (Wednesday) e sexta-feira (Friday).

* Agrupe os usuários por cidade.
* Compare o número de músicas tocadas por cada grupo na segunda, quarta e sexta.


Execute cada cálculo separadamente.

O primeiro passo é avaliar a atividade dos usuários em cada cidade. Não se esqueça das etapas "divisão-aplicação-combinação" sobre as quais falamos anteriormente na lição. Agora seu objetivo é agrupar os dados por cidade, aplicar o método de contagem apropriado durante a etapa de aplicação e então encontrar o número de músicas tocadas por cada grupo, especificando a coluna para a qual você quer obter a contagem.

Veja um exemplo de como o resultado final deve ser:
`df.groupby(by='....')['column'].method()` Execute cada cálculo separadamente.

Para avaliar a atividade dos usuários em cada cidade, agrupe os dados por cidade e encontre o número de músicas reproduzidas em cada grupo.



```python
# Contando as músicas tocadas em cada cidade
import pandas as pd

df=pd.read_csv ('/datasets/music_project_en.csv')

novas_colunas = []

for coluna in df.columns:
        coluna_striped=coluna.strip().lower().replace(' ','_')
        novas_colunas.append(coluna_striped)
df.columns = novas_colunas

df.groupby('city')['track'].count()
```

```
Saída:
city
Shelbyville    19302
Springfield    44434
Name: track, dtype: int64
```

Springfield tem uma alta atividade entre os usuários. O que posso concluir é que os usuários são mais ativos nessa cidade ou há mais dados coletados nela.

Agora vamos agrupar os dados por dia da semana e encontrar a quantidade de músicas tocadas na segunda, quarta e sexta-feira. Use a mesma abordagem que antes, mas agora precisamos agrupar os dados de uma forma diferente.


```python
# Calculando as músicas escutadas em cada um desses três dias
import pandas as pd

df=pd.read_csv ('/datasets/music_project_en.csv')

novas_colunas = []

for coluna in df.columns:
        coluna_striped=coluna.strip().lower().replace(' ','_')
        novas_colunas.append(coluna_striped)
df.columns = novas_colunas

df.groupby('day')['track'].count()
```

```
Saída:
day
Friday       22768
Monday       22106
Wednesday    18862
Name: track, dtype: int64
```

Noto que ha uma atividade maior entre os usarios na sexta. Deve ser por que esta proximo do fim de semana ;) 

Você acabou de aprender como contar entradas agrupando-as por cidade ou por dia. E agora você precisa escrever uma função que possa contar entradas simultaneamente com base em ambos os critérios.

Crie a função `number_tracks()` para calcular o número de músicas tocadas em um determinado dia **e** em uma determinada cidade. A função deve aceitar dois parâmetros:

- `day`: um dia da semana pelo qual precisamos filtrar os dados. Por exemplo, `'Monday'`.
- `city`: uma cidade pela qual precisamos filtrar os dados. Por exemplo, `'Springfield'`.

Dentro da função, você vai aplicar uma filtragem consecutiva com indexação lógica.

Primeiro, filtre os dados por dia e então filtre a tabela resultante por cidade.

Depois de filtrar os dados usando os dois critérios, conte o número de valores na coluna 'user_id' da tabela resultante. O resultado da contagem representará o número de entradas que você quer encontrar. Armazene o resultado em uma nova variável e imprima-o.

```python
def number_tracks(day, city):
   
    filtered_by_day = df[df['day'] == day]
    
    
    filtered_by_city = filtered_by_day[filtered_by_day['city'] == city]
    
   
    user_count = filtered_by_city['userid'].count()
    
    
    return user_count
```

Chame a função `number_tracks()` seis vezes, mudando os valores dos parâmetros, para que você possa recuperar os dados de ambas as cidades para cada um dos três dias.

```python
# a quantidade de músicas tocadas em Springfield na segunda-feira
number_tracks('Monday', 'Springfield')
```

```
Saída:
16715
```

```python
# a quantidade de músicas tocadas em Shelbyville na segunda-feira
number_tracks('Monday', 'Shelbyville')
```

```
Saída:
5982
```

```python
# a quantidade de músicas tocadas em Springfield na quarta-feira
number_tracks('Wednesday', 'Springfield')
```

```
Saída:
11755
```

```python
# a quantidade de músicas tocadas em Shelbyville na quarta-feira
number_tracks('Wednesday', 'Shelbyville')
```

```
Saída:
7478
```

```python
# a quantidade de músicas tocadas em Springfield na sexta-feira
number_tracks('Friday', 'Springfield')
```

```
Saída:
16890
```

```python
# a quantidade de músicas tocadas em Shelbyville na sexta-feira
number_tracks('Friday', 'Shelbyville')
```

```
Saída:
6259
```

Analisando os dados, vemos que:

Sexta-feira está consolidada como o dia com mais atividade entre os ouvintes, somando 22.768 músicas tocadas.
A cidade de Springfield registra o maior número de ouvintes ativos, com 44.434 usuários ouvindo faixas nesses 3 dias.

Essas informações sustentam a hipótese de que a atividade do usuário muda de acordo com o dia da semana e a cidade. A hipótese não pode ser rejeitada.

[Voltar ao Índice](#back)

# Conclusões <a id='end'></a>

Neste projeto, analisei os dados extraídos de um DataFrame contendo informações sobre o comportamento de usuários ao ouvir música nas cidades de Springfield e Shelbyville, com o objetivo de testar a hipótese de que existem diferenças no consumo musical entre essas duas cidades.
Antes da análise, foi necessário realizar um pré-processamento dos dados, que incluiu: padronizar os nomes das colunas,remover valores ausentes e eliminar linhas duplicadas explícitas e corrigir duplicatas implícitas.
O processo de limpeza e padronização dos dados foi essencial para garantir que essas conclusões fossem baseadas em informações confiáveis e consistentes.

### Importante
Em projetos de pesquisas reais, o teste estatístico de hipóteses é mais preciso e quantitativo. Observe também que conclusões sobre uma cidade inteira nem sempre podem ser tiradas a partir de dados de apenas uma fonte.

Você aprenderá mais sobre testes de hipóteses no sprint sobre a análise estatística de dados.

[Voltar ao Índice](#back)
