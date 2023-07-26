## Visão Geral 

Neste arquivo será apresentado alguns conceitos e anotações importantes que foram apresentados no curso.

### Importando dados com Pandas

Para importar diferentes dados com Pandas, é necessário aplicar uma função para cada tipo, são elas:

- pd.read_csv(): Para arquivos CSV (.csv), que são dados separados por virgulas; 

- pd.read_excel(): Para ler arquivos em Excel (.xls e .xlsx);

- pd.read_json(): Para ler arquivos JSON (JavaScript Object Notation), que são arquivos no formato de objeto JavaScript;

- pd.read_html(): Para ler tabelas em HTML, que são estruturas de dados organizadas em formato de tabela em uma página web;

- pd.read_sql(): Para ler banco de dados relacional, como o MySQL, PostgreSQL E SQL Server.


### Estrutura de dados DataFrame e Series

O DataFrame é uma tabela de dados bidimensional, semelhante a uma planilha do Excel, com colunas rotuladas e linhas numeradas.

A Series é uma estrutura de dados unidimensional que pode armazenar qualquer tipo de dados, rotulados com índices.


###  Análise Exploratório de Dados (EDA)

Nesta etapa, nós como cientista de dados devemos buscar entender como estão estruturados os dados que queremos analisar. Tem um carater investigativo, para compreender varias caracteristicas da base de dados.

Aqui apresento alguns tópicos das análises que podem ser feitas na etapa de EDA (Análise Exploratória de Dados):

- Resumo estatístico: Calcular estatísticas descritivas básicas para as variáveis numéricas.

- Visualização de dados: Criar gráficos e visualizações para representar a distribuição das variáveis.

- Tratamento de valores ausentes: Identificar e lidar com dados faltantes de forma apropriada.

- Análise de correlação: Avaliar a relação entre diferentes variáveis.

- Identificação de outliers: Investigar a presença de valores extremos nos dados.

- Segmentação e agrupamento: Dividir os dados em grupos ou segmentos relevantes.

- Análise de tendências ao longo do tempo: Se houver informações temporais, analisar tendências.

- Distribuição de variáveis categóricas: Analisar a distribuição de variáveis categóricas e suas frequências.

- Análise de dados geoespaciais (se aplicável): Explorar padrões geoespaciais.

- Análise de relações entre variáveis: Examinar as relações entre diferentes variáveis.

### Metodo GroupBy

Ele permite que você agrupe um DataFrame por uma ou mais colunas, criando grupos separados para cada valor único nas colunas escolhidas.

Na minha opnião, o melhor metodo de agrupar pelo GroupBy, é:

1. Defina as colunas que quer utilizar em uma lista
            col = ['Tipo','Valor']
2. Aplique o metodo .loc[] para filtrar a base de dados
            df.loc[:,col]
            ## O metodo acima faz uma filtragem pegando todas as linhas (:) apenas das colunas que foram definidas na variaveis col
3. Aplique o metodo groupby
            df.loc[:,col].groupby(['Tipo']).mean()
            ## Quando for agrupar 'POR' alguma variavel, defina entre chaves dentro do metodo groupby, após isso defina como quer calcular o restante das colunas (mean(), sum(), count(), min(), max())
4. Organize do menor para o maior, resete o index para ficar no formato de tabela mais organizada
            df.loc[:,col].groupby(['Tipo']).mean().sort_values('Valor').reset_index()

Para melhor visualização, é interessante plotar em um gráfico:

1. Defina a uma variavel todo o código do groupby
            df_preco_tipo  = df.loc[:,col].groupby(['Tipo']).mean().sort_values('Valor').reset_index()
2. Plote o gráfico da seguinte maneira:
            df_preco_tipo.plot(x='Tipo',kind='barh', figsize=(14,10), color='red');
            ## Onde x='Coluna do agrupamento', kind='Tipo do gráfico, figsize=(tamanhoX,tamanhoY), color='cor em ingles'

### Metodo Query

Esse método é usado para filtrar um DataFrame com base em condições especificadas por uma string de consulta.

Ex: 
    # Filtrando com base em valores específicos:
    df.query('Idade == 25')

    # Filtrando com base em operadores lógicos:
    df.query('Idade > 25 and Salario < 6000')

    #Filtrando usando variáveis como valores de consulta:
    idade_alvo = 30
    df.query(f'Idade == {idade_alvo}')

    #Usando o método inplace para alterar o DataFrame original:
    df.query('Salario > 4000', inplace=True)
    print(df)

    #Usando colunas com espaços ou caracteres especiais:
    data = {'Nome Completo': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
        'Idade Atual': [25, 30, 22, 27, 35],
        'Salário Atual': [4000, 4500, 3800, 5200, 6000]}
    df = pd.DataFrame(data)
    # Filtrando apenas as pessoas com idade maior que 25
    resultado_query = df.query('`Idade Atual` > 25')

    #Usando listas
    imoveis_comerciais = ['Conjunto Comercial/Sala','Prédio Inteiro','Loja/Salão', 'Galpão/Depósito/Armazém','Casa Comercial',
                      'Terreno Padrão', 'Loja Shopping/ Ct Comercial','Chácara','Loteamento/Condomínio', 'Sítio', 'Pousada/Chalé', 
                      'Studio', 'Hotel', 'Indústria' ]
    df_raw.query('@imoveis_comerciais in Tipo')


### Metodo value_counts()

Utilizando quando precisamos contar quantas vezes cada variavel de uma coluna aparece na nossa base de dados.

Ex: df.Tipo.value_counts()

    out: Tipo
        Apartamento           19532
        Casa de Condomínio      996
        Casa                    967
        Quitinete               836
        Flat                    476
        Casa de Vila            249
        Box/Garagem              82
        Loft                     51

- É possivel gerar também os valores em percentual no metodo value_counts(), basta definir o parametros normalize=True
    df.Tipo.value_counts(normalize=True)

- Para melhor a visualização, podemos transformar em tabela, organizar do menor para o maior e resetar o index
    df.Tipo.value_counts(normalize=True).to_frame().sort_values('proportion').reset_index()

**Outros parametros para a função**
- salvando o dataframe em uma variável
df_exemplo = df['Tipo'].value_counts(normalize=True).to_frame().sort_values('Tipo')

- alterando o nome da coluna "Tipo" para "Percentuais"
df_exemplo.rename(columns={'Tipo': 'Percentuais'}, inplace=True)

- visualizando o dataframe
df_exemplo 


