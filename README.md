# **Analize de banco de dados com PYTHON**

## Introdução

Este projeto tem como objetivo praticar e aprender conceitos da analize de dados com PYTHON.

### Passo a passo do projeto

0. Entender o problema a ser resovido
1. Importar a base de dados
2. Visualizar a base de dados
3. Identificar problemas na base e corrigilos
4. Realizar a analize inicial
5. Analize completa

### Bibliotecas utilizadas

- Pandas
- Plotly

Segue abaixo respectivamente os códigos para instalar as bibliotecas, caso não as tenha instalado:

```
pip install pandas

pip install plotly
```

## Código explicado
Partindo do préssuposto que já entendemos o que precisamos fazer para ajudar a empresa, vamos por partes. Começando pelo passo 1.

1. Importar a base de dados: Para importar a base utilizamos a biblioteca pandas. Em seguida importamos o a base com o comando **pd.read_csv()**

```
import pandas as pd 

Base1 = pd.read_csv('Bases de dados/cancelamentos_sample.csv')
```

2. Visualizar a base de dados: Usaremos o comando abaixo

``` 
display(Base1)
```
3. Identificar problemas na base e corrigilos: Porque isso? Bom todo base de dados tem erros, como espaços em branco (veremos essa parte no passo 4) e colunas com informações irrelevantes para a analize. E no nosso caso identificamos que a coluna do ID do cliente não iranos ajudar. Então utilizaremos o comando **drop**. Como abaixo e, em seguida visualizamos a base novamente

```
Base1 = Base1.drop(columns= 'CustomerID')

display(Base1)
```

4. Identificar os espaços em branco: Para conseguirmos identificar quantos espaços em branco podemos utilizar a função **info()**. Como a nossa base de dados é grande podemos realizar a exclusão das linhas da base que possuem valores vazios, utilizando o comando **dropna**

```
display(Base1.info())

Base1 = Base1.dropna()

#Caso o banco seja muito pequeno se pode realizar a
#edição dos valores em branco ex:
#Base1 = Base1.fillna()

display(Base1.info())
```

5. Realizar a analize inicial: Para essa parte vamos descobrir quantas pessoas cancelaram e quantas pessoas não cancelaram e, para selecionar a coluna utilizamos os **[ ]**. Utilizaremos a função **value_counts** e dentro do parentezes passremos o valor **normalize= True**

```
display(Base1['cancelou'].value_counts(normalize= True))
```

6. Analize completa: A partir desta parte utilizaremos gráficos. Para isso importaremos a biblioteca **plotly**, usando um laço **for** para fazer de forma dinamica um gráfico para cada coluna da base. E o modelo de gráfico utilizado será o **histogram**, passando para ele a **Base de dados**, quem será o **x** dele e separar por cor quem cancelou com **color** e, em seguida utilizamos a função **show()** para criar os gráficos

```
import plotly.express as px

for coluna in Base1.columns:
    grafico = px.histogram(Base1, x= coluna, color= 'cancelou')

    grafico.show()
```

7. Parte final: Após analizarmos os gráficos gerados (olhando as informações que saltão os olhos), nós irremos editar a base de dados de acordo com essas informações. Como são varias colunas que serão alteradas utilizaremos os **[ ]** que definem listas em PYTHON. Em seguinda selecionamos a coluna e passamos a condição. Para fazer mais alterações com colunas distintas na mesma linha de código se pode adicionar **&**. Posterior a essa parte exibimos as informações no formato de porcentagem com a função **map()**

```
Base1 = Base1[(Base1['duracao_contrato'] != 'Monthly') & 
            (Base1['ligacoes_callcenter'] <= 4.0) & 
            (Base1['dias_atraso'] <= 20.0)]

display(Base1['cancelou'].value_counts(normalize= True).map('{:.1%}'.format))
```

## Conclusões da analize

Nesta analize descobrimos que essa empresa em questão estava precisando de ajuda urgentemente, mais da metade dos clientes estavam cancelando.  
Atravez dessa analize descobrimos que:
- Clientes do plano mensal todos cancelam
- Clientes que ligam mais de 4 vezes no Call Center cancelam
- Clientes que atrazaram o pagamento mais de 20 dias cancelam

As medidas para resolver essas situações seriam respectivamente:

- Oferecer desconto nos planos trimestrais e anuais. Ou excluir o plano mensal 
- Criar uma metodologia de atendimento que resolva a dificuldade o cliente em no maxímo 3 ligações
- Cliente atrazou um dia, já entrar em contato com ele, 3 dias entrar em contato novamente. Resvolvendo em até 10 dias

