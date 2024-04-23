# Primeiros passos com Snowflake e DBT Cloud:

O real objetivo deste trabalho é demonstrar, passo a passo, como utilizar o DBT Cloud (Data Build Tool) e o SNOWFLAKE em um projeto analítico de dados.
  
SNOWFLAKE é a nuvem de dados que permite criar aplicativos com uso intensivo de dados sem sobrecarga operacional, para que você possa se concentrar em dados e análises em vez de no gerenciamento de infraestrutura.

DBT é um fluxo de trabalho de transformação de dados seguindo as melhores práticas de engenharia de software. Utilizando o SQL é possível construir pipelines de dados de nível de produção. Ele transforma os dados no warehouse aproveitando plataformas de dados em nuvem como o SNOWFLAKE.


## 🧪 Manual de Apoio
O projeto foi desenvolvido com base nas instruções dos seguintes materiais:

- [Manual Base](https://quickstarts.snowflake.com/guide/accelerating_data_teams_with_snowflake_and_dbt_cloud_hands_on_lab/index.html?index=..%2F..index#0)

- [Vídeo de apoio](https://www.youtube.com/watch?v=84RA7TuhCpg)

## 🔨 Ferramentas Necessárias
Para iniciar um projeto utilizando o DBT CLOUD, usaremos uma conta de teste. A criação da conta DBT CLOUD será feita por meio do SNOWFLAKE através de uma conexão nativa (parceira). 
 
Por meio desta conta, será possível construir pipelines de transformação de dados escaláveis usando o DBT e o SNOWFLAKE.

- [Link de criação conta teste Snowflake](https://signup.snowflake.com/)

## 🚀 Inicializando o projeto DBT
Com a conta teste criada no Snowflake é preciso verificar que possuimos todos os dados de que precisamos para transformar e executar o projeto. 

Vá para a guia **Worksheets**, clique na opção de **+Worksheets** para adicionar uma nova planilha. Neste momento, clique em **Databases**, você verá dois banco de dados e um deles se chama **snowflake sample data**. Ao explandi-ló  você verá o schema **tpch sf1** e a partir daí tem-se todas as tabelas que possuem os dados brutos que serão transformados pelo projeto.

Para ter certeza, vamos exibir alguns resultados destas tabelas. Crie um **warehouse** de amostra e com a capacidade de tamanho pequena com a seguinte linha de comando:
  
```bash
create warehouse sample_warehouse with warehouse_size = xsmall
```

No quadro superior a direita, certifique de indicar a role **ACCOUNTADMIN** e o warehouse **SAMPLE_WAREHOUSE**, em seguida, execute o seguinte trecho de comando: 
  
```bash
select *
from snowflake_sample_data.tpch_sf1.orders
limit 100;
```
Os resultados serão exibidos na sequência.

Com isso, é possível criar uma conta  DBT Labs através da partner connect. Pesquise por DBT em partner connect e selecione o item da pesquisa, no pop-up aberto, na seção **Optional Grant** informe o nome do banco de dados criado **SNOWFLAKE_SAMPLE_DATA**. Siga com a conexão e ative-a.

Você será redirecionado para o site do DBT. Crie uma conta neste ambiente, clicando em **Complete Registration**. Agora você deve ser redirecionado para sua conta DBT Cloud já com uma conexão com sua conta SNOWFLAKE.

Nesta etapa, para entrar na área de nuvem DBT, abra a IDE clicando no botão **Develop** no canto superior a esquerda. Quando o IDE terminar de carregar, clique no **Initialize dbt project**, botão verde no canto superior esquerdo da tela. O processo de inicialização cria um projeto DBT na árvore de arquivos no lado esquerdo da tela com todos os arquivos e pastas principais do DBT necessários. 

Após o projeto criado, clique **Commit and sync**, no canto superior esquerdo, para enviar/submeter os novos arquivos e pastas da etapa de inicialização. Um pop-up será aberto, informe uma mensagem de commit relevante para o trabalho: **initialize project**. Na sequencia, clique em **Commit Changes** na janela pop-up.

_Commit criará uma nova versão com as edições acrescentadas ao projeto, gerando uma subversão do projeto. Push irá direcionar essa alteração da branch master para origin, ou seja, retirar do repositório local (Git) para o repositório remoto (GitHub)._
 
_Cada Commit dado, será criada uma nova versão, ao final do dia, dá-se o push para gerar uma cópia do projeto no GitHub (Origin)._

Ao confirmar, ele será salvo no repositório git gerenciado que foi criado durante a inscrição no Partner Connect. Este commit inicial é o único commit que será feito diretamente em nosso main branch (branch master/ramificação principal) e daqui em diante estaremos fazendo todo o nosso trabalho em um branch de desenvolvimento (merge no master). Isso nos permite manter nosso trabalho de desenvolvimento separado do código de produção.

A IDE possui alguns recursos chaves, ela é um editor de texto, um executor SQL e uma CLI com controle de versão git, tudo integrado em um único pacote. O fluxo de trabalho git no DBT Cloud permite controlar facilmente a versão de todo o seu trabalho com apenas alguns cliques. 

Clique em **+Create new file** para abrir o notebook da IDE. Nesta fase, um SQL Runner é aberto, uma linha de comando inferior na tela para executar comandos do DBT como ```dbt run``` e uma árvore de arquivos e controles git no canto superior a esquerda da tela.

![Imagem](https://quickstarts.snowflake.com/guide/accelerating_data_teams_with_snowflake_and_dbt_cloud_hands_on_lab/img/516d62e53cdc1ba1.png)

Agora é possível executar os primeiros modelos DBTs, os quais vem por padrão na pasta **models\example**, para ilustrar como executar o dbt na linha de comando. Digite ```dbt run``` na linha de comando na parte inferior da tela e pressione **Enter** no teclado. Quando a barra de execução se expandir, você poderá ver os resultados da execução concluidos com sucesso.

![Imagem](https://quickstarts.snowflake.com/guide/accelerating_data_teams_with_snowflake_and_dbt_cloud_hands_on_lab/img/7c1cb782586306f4.png)

Os resultados da execução permitem que você veja o código que o DBT compila e envia ao SNOWFLAKE para execução. Para visualizar os logs, clique em uma das guias do modelo e, em seguida, clique em **details**. Se você rolar um pouco para baixo, poderá ver o código compilado e como o DBT interage com o SNOWFLAKE. Dado que esta execução ocorreu em nosso ambiente de desenvolvimento, os modelos foram criados em seu esquema de desenvolvimento, estruturado como sua inicial e sobrenome:
```
PC_DBT_DB.dbt_PFernandes.my_first_dbt_model
PC_DBT_DB.dbt_PFernandes.my_second_dbt_model
```

Agora vamos passar para o Snowflake para confirmar se os objetos foram realmente criados. Retorne ao SNOWFLAKE, em Data -> Databases, clique em **refresh** e desça a árvore do bando de dados **PC_DBT_DB**. Neste, deverá haver o schema **dbt_PFernandes** que, por sua vez, deverá conter as duas tabelas criadas: **my_first_dbt_model** e **my_second_dbt_model**.

Agora, no worksheet do Snowflake, crie um novo **warehouse** que usaremos no projeto dbt. Copie e cole a seguinte série de comandos e execute-os em ordem no worksheet do Snowflake.

```
	use role accountadmin;
	create warehouse pc_dbt_wh_large with warehouse_size = large;
	grant all on warehouse pc_dbt_wh_large to role pc_dbt_role;
```

Aqui estamos usando a função de administrador de conta para criar um tamanho de **warehouse** maior e concedendo todas as habilidades para ler, escrever e todas as demais atribuições.

Após executar, no quadro superior a direita, certifique de indicar a role **ACCOUNTADMIN** e o warehouse **PC_DBT_WH_LARGE**.

Podemos, então, seguir para a próxima etapa de definir a estrutura fundamental do projeto. 

Para definir a base para este projeto vamos retornar ao **DBT Cloud** e criar um novo **branch** (ramificação). Clique no **create branch**, um botão verde no canto superior esquerdo da tela para abrir uma janela e nomear sua filial (snowflake_workshop). Nesta fase, estaremos trabalhando no **branch** de desenvolvimento. 

O próximo passo é abrir o arquivo **dbt_project.yml** do projeto. Este é o arquivo que o dbt procura para reconhecer que o diretório de arquivos em que estamos trabalhando é um projeto DBT. Nele, vamos atualizar seus parâmetros substituindo quase todo o conteúdo existente, deixando apenas o trecho: 

```
# In dbt, the default materialization for a model is a view. This means, when you run 
# dbt run or dbt build, all of your models will be built as a view in your data platform. 
# The configuration below will override this setting for models in the example folder to 
# instead be materialized as tables. Any models you add to the root of the models folder will 
# continue to be built as views. These settings can be overridden in the individual model files
# using the `{{ config(...) }}` macro.

models:
  my_new_project:
    # Applies to all files under models/example/
		example:
		  materialized: view
```

Copie e cole o seguinte código acima do trecho que devemos manter no arquivo:

```
name: 'snowflake_workshop'
version: '1.0.0'
config-version: 2

profile: 'default'

source-paths: ["models"] # A MINHA VERSÃO EXIGIU ESSA CONFIGURAÇÃO model_paths: ["models"]
analysis-paths: ["analysis"] # A MINHA VERSÃO EXIGIU ESSA CONFIGURAÇÃO analysis-paths: ["analyses"]
test-paths: ["tests"]
seed-paths: ["seeds"]
macro-paths: ["macros"]
snapshot-paths: ["snapshots"]

target-path: "target"  
clean-targets:         
  - "target"
  - "dbt_modules"

models:
  snowflake_workshop:
    staging:
      materialized: view
      snowflake_warehouse: pc_dbt_wh
    marts:
      materialized: table
      snowflake_warehouse: pc_dbt_wh_large
```

As principais configurações no arquivo estão na seção de modelos. Aqui estamos definindo configurações que queremos aplicar a todos os modelos dentro dessa pasta. Especificamente, estamos demonstrando:

_a configuração de **materialized**, que informa ao dbt como materializar modelos ao compilar o código antes de enviá-lo para o Snowflake, e
a configuração do **snowflake_warehouse**, que especifica qual warehouse do Snowflake deve ser usado ao construir modelos no Snowflake._

A materialização define a estratégia que o _DBT_ deverá usar ao criar as tabela dentro do Snowflake. Geralmente são **table** e **view** as mais comuns. Por padrão, todos os modelos são materializados como **view** e outros tipos de visualizações podem ser configuradas no arquivo _.yaml_ do projeto.

O benefício de usar materializações _dbt_ é que quando você executa um modelo _dbt_, o _dbt_ usará o _DDL_ correto associado à materialização para criar o equivalente do modelo no data warehouse.

Cada conexão _dbt_ declara um _warehouse_ padrão para usar ao construir modelos no Snowflake. Com a configuração **snowflake_warehouse**, o _dbt_ usará o warehouse x-small **pc_dbt_wh** para construir tudo na pasta de teste e o warehouse large **pc_dbt_wh_large** para construir tudo na pasta marts. Essa configuração nos permite aumentar e diminuir o tamanho do warehouse com base nas necessidades de computação de diferentes partes do projeto.

A próxima fase iremos criar uma estrutura de pastas dentro da pasta **models**, clicando nos três pontinhos **Create Folder**. Digite ``staging/tpch`` e a pasta **staging** será criada, dentro dela a pasta **tpch** que armazenará os dados TPCH.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/a33d922e-877f-41b0-9022-6232cf655380)

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/0d91054a-9959-4b93-b6fb-2001401917ed)

Vamos criar nossas duas pastas finais da mesma maneira. Passe o cursor sobre o subdiretório **models**, clique nos três pontos que aparecem à direita e clique em **Create Folder**. Desta vez, na janela pop-up, você inserirá o seguinte caminho de arquivo: ```marts/core```. Aqui estamos criando uma pasta **marts** dentro **models** e depois uma pasta **core** dentro de **marts**. Sua árvore de pastas deve ficar assim quando tudo estiver dito e feito: 

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/b176bbdb-6903-47d1-b3fc-dcf9ddb4cb73)

Essas pastas serão usadas para armazenar os modelos de transformação e nossa preparação. 

A última coisa que faremos para configurar o projeto _DBT_ é implementar pacotes. **dbt packages** são projetos _DBT_ que você pode trazer para o seu projeto e usar como se fossem seus. Muitos pacotes são hospedados no **dbt Packages Hub** e fornecem a capacidade de gerar código reutilizável para criar fontes e modelos _DBT_, acessar uma variedade de macros úteis em **jinja** e criar restrições de banco de dados para seus bancos Snowflake.

_Jinja é uma linguagem de modelagem python que você pode usar no dbt para fazer coisas que normalmente não são possíveis com SQL. Alguns exemplos incluem a construção de estruturas de controle como ```if``` (instruções) e ```for``` (loops) e a abstração de trechos de código em macros reutilizáveis ​​para que você não se repita (DRY - Don't Repeat Yourself)._

_Uma macro é um pedaço de código jinja que pode ser reutilizado várias vezes em um projeto dbt, como uma função em outras linguagens de programação. Algumas das funções mais importantes do dbt, como ```source``` e  ```ref```, também são macros (abordaremos isso mais tarde no laboratório)._

Vamos adicionar o pacote **dbt_utils** ao projeto. Há alguns lugares onde usaremos macros do pacote **dbt_utils** para nos ajudar a escrever código DRY. Para instalar o pacote, primeiro crie um novo arquivo em seu diretório inicial (mesmo nível do seu arquivo **dbt_project.yml**) e chame-o de **packages.yml**. Em seguida, copie e cole o código nele:

```
packages:
	- package: dbt-labs/dbt_utils
	version: 0.8.4
```

Salve o arquivo. Neste momento o DBT executará automaticamente o dbt deps, caso ele não faça, execute o trecho ```dbt deps``` na linha de comando.

Essa ação que informa ao _DBT_ para instalar os pacotes definidos em seu arquivo **packages.yml**. Clique em "Enter" e você verá uma mensagem de sucesso quando for concluído.

Até aqui estamos desenvolvendo no repositório local, vamos reservar um segundo aqui para submeter o trabalho antes de passar para a próxima etapa. Clique no botão **Commit and sync**, escreva uma mensagem de commit como **setup structure for project** e clique em **Commit Changes** para confirmar as mudanças feitas.

Na próxima etapa, usaremos as tabelas do Snowflake **orders** e **lineitem** do esquema **TPCH_SF1** para nossas transformações e vamos criar essas tabelas como fontes no projeto.

_Fontes: As fontes em dbt permitem nomear e descrever dados brutos de seu warehouse em seu projeto dbt, permitindo estabelecer a linhagem de dados brutos para modelos transformados._

_Modelos de teste: Os modelos de preparo têm um relacionamento 1:1 com tabelas de origem. É aqui que você realizará transformações simples, como renomear, reformular ou alterar uma coluna para um formato mais utilizável. A construção de modelos de preparação fornece uma base clara para a modelagem downstream e permite a modelagem modular DRY._

Crie um novo arquivo chamado **tpch_sources.yml** com o seguinte caminho de arquivo: ``models/staging/tpch/tpch_sources.yml``. Este arquivo será usado para definirmos nossas fontes do projeto, as tabelas  **orders** e **lineitem**. Em seguida, cole o seguinte código no arquivo antes de salvá-lo:

```
version: 2
sources:
	- name: tpch
	description: source tpch data
	database: snowflake_sample_data
	schema: tpch_sf1
	tables:
	  - name: orders
		description: main order tracking table
		columns:
		  - name: o_orderkey
			description: SF*1,500,000 are sparsely populated
			tests: 
			  - unique
			  - not_null

		  - name: lineitem
			description: main lineitem table
			columns:
			  - name: l_orderkey
				description: Foreign Key to O_ORDERKEY
				tests:
				  - relationships:
					  to: source('tpch', 'orders')
					  field: o_orderkey
	
```

No arquivo, você pode ver que definimos o banco de dados **snowflake_sample_data** de onde iremos obter as informações, o esquema **tpch_sf1** e ambas as tabelas com as quais iremos construir e transformar. Abaixo de cada nome de tabela temos descrições e testes que aplicamos às nossas fontes.

Agora que temos as fontes instaladas, o que vamos fazer é criar e configurar os **modelos de teste** para as duas fontes de dados. Estes serão arquivos _.sql_. Dado o relacionamento individual entre os modelos de teste e suas tabelas de origem correspondentes, construiremos dois modelos de preparo.

Vamos começar com a tabela de pedidos **(orders)**. Crie um novo arquivo chamado **stg_tpch_orders.sql** com o seguinte caminho de arquivo: _models/staging/tpch/stg_tpch_orders.sql_. Em seguida, cole o seguinte código no arquivo antes de salvá-lo:

```
with source as (
    select * from {{ source('tpch', 'orders') }}
),

renamed as (
    select
        o_orderkey as order_key,
        o_custkey as customer_key,
        o_orderstatus as status_code,
        o_totalprice as total_price,
        o_orderdate as order_date,
        o_orderpriority as priority_code,
        o_clerk as clerk_name,
        o_shippriority as ship_priority,
        o_comment as comment
    from source
)

select * from renamed
```

Tudo o que estamos fazendo aqui é puxar os dados de origem para o modelo usando a função **source** e renomear algumas colunas. Isso serve como ponto de partida para todos os outros modelos que precisam fazer referência a esses dados para que a nomenclatura permaneça consistente em todo o projeto.

Para o segundo modelo, criaremos outro arquivo na mesma pasta chamada **stg_tpch_line_items.sql** com o seguinte caminho de arquivo: _models/staging/tpch/stg_tpch_line_items.sql_. Em seguida, cole o seguinte código nele antes de salvar o arquivo:

```
with source as (
    select * from {{ source('tpch', 'lineitem') }}
),

renamed as (
    select  
        {{dbt_utils.surrogate_key (['l_orderkey', 'l_linenumber'])}} as order_item_key,
        l_orderkey as order_key,
        l_partkey as part_key,
        l_suppkey as supplier_key,
        l_linenumber as line_number,
        l_quantity as quantity,
        l_extendedprice as extended_price,
        l_discount as discount_percentage,
        l_tax as tax_rate,
        l_returnflag as return_flag,
        l_linestatus as status_code,
        l_shipdate as ship_date,
        l_commitdate as commit_date,
        l_receiptdate as receipt_date,
        l_shipinstruct as ship_instructions,
        l_shipmode as ship_mode,
        l_comment as comment
    from source
)

select * from renamed
```

Aqui temos uma renomeação semelhante ao que fizemos no primeiro **modelo de teste**, bem como a adição de uma nova coluna utilizando o pacote **dbt_utils**. O **dbt_utils.surrogate_key** cria uma chave substituta com hash que chamamos **order_item_key** usando as colunas listadas na macro. A implementação dessa chave substituta nos dá uma coluna exclusiva que podemos usar para testar facilmente a exclusividade posteriormente.

Ambos arquivos modelos criados, serão a base de todos os demais modelos para tratativas futuras. Aqui, apenas padronizaremos as fontes que iremos utilizar, a nomeclatura das colunas que vamos chamar e a tratativa minima do formato dos dados como, **datetime** para **date**.

A função **source** atribuida em ambos os arquivos _.sql_ é preferencialmente usada em vez de uma referência de banco de dados codificada, por criar uma dependência entre o objeto de banco de dados de origem e o **modelos de teste**. Isso será muito importante quando dermos uma olhada em nossa linhagem de dados. Definir fontes e referenciá-las com a função **source** também permite testar e documentar essas fontes como você pode fazer com qualquer outro modelo em seu projeto que você construa sobre suas fontes. Além disso, se a sua fonte alterar o banco de dados ou o esquema, você só precisará atualizá-lo no seu arquivo **tpch_sources.yml**, em vez de atualizar todos os modelos nos quais ele pode ser usado.

Agora com os **modelos de testes** criados, é necessário construirmos eles no Snowflake, pois, até então, estamos executando as modificações localmente. Para isso, iremos executar na linha de comando o ```dbt run``` para executar todos os modelos do nosso projeto, que inclui os dois novos **modelos de teste** e os **modelos de exemplo** existentes na pasta **models/example**.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/e252982c-77c5-4569-852a-56c4000cf12a)

Volte ao Snowflake, atualize os objetos do banco de dados, abra o esquema de desenvolvimento **DBT_PFERNANDES** para confirmar se os novos modelos estão nas pastas de **table** e **view**. 

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/33700fea-2fc8-426f-b819-d5d7a66c1370)

Agora certifique-se de submeter seus novos modelos ao seu **branch git**. Clique no botão **Commit and sync** e envie uma mensagem ao seu commit **sources and staging** antes de prosseguir.

A próxima fase envolve a transformação dos dados. Aqui, são criadas as tabelas unidas que geram uma infinidade de lógica de négocio e insights integrados em um pipeline que preparamos para alimentar uma camada dupla. A camada que criará muitos insights e, geralmente, é o que o usuário final consome se integrarmos ao BI.

O plano aqui é construir dois **modelos transformados**: um é um **modelo intermediário**, que não será a visão final, para executar cálculos de novos itens de linha e o outro, será de fato chamado tabela, é um **modelo de fato** que agrega os cálculos de novos itens de linha no nível do pedido.

O processo de criação será similar ao que fizemos quando criamos os **modelos teste**, porém, faremos isso na pasta **marts**. Vamos começar criando um novo arquivo chamado **int_order_items.sql** com o seguinte caminho de arquivo: _models/marts/core/int_order_items.sql_. Em seguida, copie e cole o seguinte bloco de código no novo modelo e clique em salvar:

```
with orders as (
    select * from {{ ref('stg_tpch_orders') }}
),

line_item as (
    select * from {{ ref('stg_tpch_line_items') }}
)

select 
    line_item.order_item_key,
    orders.order_key,
    orders.customer_key,
    orders.order_date,
    orders.status_code as order_status_code,
    line_item.part_key,
    line_item.supplier_key,
    line_item.return_flag,
    line_item.line_number,
    line_item.status_code as order_item_status_code,
    line_item.ship_date,
    line_item.commit_date,
    line_item.receipt_date,
    line_item.ship_mode,
    line_item.extended_price,
    line_item.quantity,
    
    -- extended_price is actually the line item total,
    -- so we back out the extended price per item
    (line_item.extended_price/nullif(line_item.quantity, 0))::decimal(16,2) as base_price,
    line_item.discount_percentage,
    (base_price * (1 - line_item.discount_percentage))::decimal(16,2) as discounted_price,

    line_item.extended_price as gross_item_sales_amount,
    (line_item.extended_price * (1 - line_item.discount_percentage))::decimal(16,2) as discounted_item_sales_amount,
    -- We model discounts as negative amounts
    (-1 * line_item.extended_price * line_item.discount_percentage)::decimal(16,2) as item_discount_amount,
    line_item.tax_rate,
    ((gross_item_sales_amount + item_discount_amount) * line_item.tax_rate)::decimal(16,2) as item_tax_amount,
    (
        gross_item_sales_amount + 
        item_discount_amount + 
        item_tax_amount
    )::decimal(16,2) as net_item_sales_amount

from
    orders
inner join line_item
        on orders.order_key = line_item.order_key
order by
    orders.order_date
```

Na parte superior, selecionamos todos os dados dos **modelos de teste** em dois cte's (Common Table Expression/Sub função) usando a função ```ref```. A instrução principal ```select``` é unir os dois **modelos de teste**, extrair um pedaço de colunas existentes e, em seguida, realizar vários cálculos diferentes nos dados dos itens de linha. Todos os cálculos são pontos de dados que nos interessam e não foram calculados em nossa fonte de dados brutos, como preços de itens individuais com descontos e valores totais de vendas com impostos.

_```NULLIS```: esta função é utilizada para comparação de dois parâmetros, sendo os dois iguais o retorno será null, caso contrário o primeiro parâmetro será retornado._

A função ```ref``` é a função mais importante em _DBT_ e é semelhante à função de origem que usamos anteriormente. Neste caso, está fazendo referência aos nossos modelos de teste. Como regra, você deve sempre usar a função ```ref``` para se referir a qualquer modelo DBT existente ao construir modelos. Isso é importante para criar dependências entre modelos _DBT_, bem como permitir que você promova código perfeitamente entre diferentes ambientes. Quando promovermos nosso código do ambiente de desenvolvimento para o ambiente de produção posteriormente, a função ```ref``` compilará o objeto de banco de dados correto associado ao ambiente de produção com base em nossas configurações (tanto na conexão quanto no projeto).

Agora vamos criar nosso **modelo de fato** final transformado. Comece criando um novo arquivo chamado **fct_orders.sql** com o seguinte caminho de arquivo:_models/marts/core/fct_orders.sql_. Em seguida, copie e cole o seguinte código no arquivo antes de salvar:

```
with orders as (
    select * from {{ ref('stg_tpch_orders') }} 
),

order_item as (
    select * from {{ ref('int_order_items') }}
),

order_item_summary as (
    select 
        order_key,
        sum(gross_item_sales_amount) as gross_item_sales_amount,
        sum(item_discount_amount) as item_discount_amount,
        sum(item_tax_amount) as item_tax_amount,
        sum(net_item_sales_amount) as net_item_sales_amount
    from order_item
    group by 1
),

final as (
    select 
        orders.order_key, 
        orders.order_date,
        orders.customer_key,
        orders.status_code,
        orders.priority_code,
        orders.clerk_name,
        orders.ship_priority,
                
        1 as order_count,                
        order_item_summary.gross_item_sales_amount,
        order_item_summary.item_discount_amount,
        order_item_summary.item_tax_amount,
        order_item_summary.net_item_sales_amount
    from orders
	inner join order_item_summary
            on orders.order_key = order_item_summary.order_key
)

select * from final
order by order_date

```

Este modelo começa trazendo nossos dados de pedidos **(orders)** temporários **stg_tpch_orders.sql** **(modelo de teste)** e nossos dados de itens de pedido **transformados** **int_order_items** **(modelo intermediário)** usando a função ```ref``` em cada caso. 

Em seguida, ele sumariza os cálculos dos itens do pedido e agrega esses valores no nível do pedido antes de juntá-los novamente aos demais atributos do nível do pedido para obter a saída final transformada. O resultado é que podemos gerar relatórios sobre os pedidos com os descontos e valores de impostos correspondentes, o que não conseguimos fazer apenas com os dados de preparação.

_```GROUP BY 1```: significa agrupar pela primeira coluna do seu conjunto de resultados, independentemente do nome. Você pode fazer o mesmo com ORDER BY._

Agora que construímos os **modelos transformados**, vamos fazer executar o ```dbt run``` para incorporar esses modelos em nossos esquemas de desenvolvimento **DBT_PFERNANDES** no Snowflake. Desta vez, em vez de fazer ```dbt run``` e executar todos os modelos do nosso projeto, vamos dizer ao _DBT_ para construir apenas nossos novos modelos transformados. Para fazer isso usaremos o seguinte comando: ```dbt run --select int_order_items+```.

Já usamos o argumento ```--select``` para dizer ao _DBT_ para executar especificamente o modelo ou caminho que fornecemos após o argumento, neste caso, o nosso modelo **int_order_items**. 
O sinal de mais **(+)** anexado ao final de ```int_order_items``` é um operador gráfico que usa o gráfico de dependência do _DBT_ para executar todos os modelos downstream do modelo selecionado. Portanto, neste caso, o _DBT_ será executado ```int_order_items``` e todas as dependências downstream desse modelo também, e é assim que este comando executa nosso **modelo intermediário** e o **modelo final**.

Agora vamos dar uma olhada nos resultados detalhados do nosso modelo **fct_orders** para que possamos entender o que está acontecendo no código compilado. Quando criamos nosso arquivo **dbt_project.yml**, configuramos todos os modelos na pasta **marts** para serem executados usando o warehouse **pc_dbt_wh_large**, e podemos ver isso em ação no topo do log:

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/ea31815d-1c03-428a-b536-c421b4aa3e39)

Há também algumas peças sobre o próprio modelo a serem destacadas. A primeira é que o _DBT_ está agrupando nossa instrução ```select``` em _DDL_ para nós e construindo a tabela em nosso esquema de desenvolvimento **DBT_PFERNANDES**. A configuração ```materialized``` em nosso arquivo **dbt_project.yml** é responsável por construir isso como uma tabela, em oposição à materialização da visualização padrão.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/0638138f-1bda-40c0-993a-fea8bd45439e)

O outro ponto importante aqui é como as instruções ```ref``` em cada um dos dois primeiros cte **(sub função)** foram compiladas no objeto de banco de dados apropriado. Aqui você pode ver que o _DBT_ compilou o código para referenciar o modelo **stg_tpch_orders** e o modelo **int_order_items** no esquema de desenvolvimento **DBT_PFERNANDES**, visto que estamos construindo o modelo em nosso ambiente de desenvolvimento.

Agora reserve um momento para clicar no botão **commit** e enviar uma mensagem **create intermediate and fact models** para salvar suas alterações mais recentes.

Nesta fase que construímos nossos modelos e transformações, é importante documentá-los e testá-los. Os recursos nativos do _dbt_ incluem uma estrutura de documentação e teste de dados em duas estruturas: **genérico** e **singular**.

_Singular: é o teste em sua forma mais simples. Se você pode escrever uma consulta SQL que retorna linhas com falha, é um teste dbt que pode ser executado pelo comando "dbt test"._

_Genérico: possuem a capacidade de aceitar argumentos. Você os define em um bloco de teste e pode referenciá-los por nome em seus arquivos .yml (aplicando-os a modelos, colunas, fontes, instantâneos e sementes). O dbt vem com quatro testes genéricos que funcionam imediatamente: unique, not null, accepted values, e relationships._

**Testes genéricos** e descrições para documentação são definidos em arquivos _YAML_. Para começar, criaremos um novo arquivo chamado **core.yml** com o seguinte caminho de arquivo: _models/marts/core/core.yml_. Copie e cole o seguinte bloco de código no novo arquivo e salve:

```
version: 2

models:
  - name: fct_orders
    description: orders fact table
    columns:
      - name: order_key
        description: primary key of the model
        tests:
          - unique
          - not_null
          - relationships:
              to: ref('stg_tpch_orders')
              field: order_key
              severity: warn
      - name: customer_key
        description: foreign key for customers
      - name: order_date
        description: date of the order
      - name: status_code
        description: status of the order
        tests:
          - accepted_values:
              values: ['P','O','F']
      - name: priority_code
        description: code associated with the order
      - name: clerk_name
        description: id of the clerk
      - name: ship_priority
        description: numeric representation of the shipping priority, zero being the default
      - name: order_count
        description: count of order
      - name: gross_item_sales_amount
        description: '{{ doc("gross_item_sales_amount") }}'
      - name: item_discount_amount
        description: item level discount amount. this is always less than or equal to zero
      - name: item_tax_amount
        description: item level tax total
      - name: net_item_sales_amount
        description: the net total which factors in discount and tax
```

Este arquivo está no diretório _models/marts/core_ contém testes e descrições para os modelos neste diretório, neste caso, para **fct_orders**. Como prática recomendada, recomendamos ter pelo menos um arquivo _YAML_ para teste e documentação por diretório.

Ao observar a formatação, o nível superior é o modelo que estamos descrevendo e fornecemos todas as informações diretamente abaixo do nome do modelo. No nosso caso, fornecemos uma descrição para o nosso modelo.

Descendo para o nível da coluna, estamos utilizando todos os **testes genéricos** em uma variedade de colunas. Esperamos que a coluna __order_key__ seja ```unique``` e ```not_null```. O teste ```relationships``` está verificando se o **order_key** neste modelo também existe como um _ID_ no **stg_tpch_orders**. E, finalmente, há um teste ```accepted values``` na coluna _status_code_ para garantir que os valores gerados nessa coluna atendam às nossas expectativas.

Sobre as descrições, elas estão divididas entre duas notações diferentes, uma usando texto simples e outra usando uma **referência jinja** a um bloco de documento. A primeira funciona exatamente como no nível do modelo e permite documentar qualquer coluna no arquivo _YAML_. O bloco _doc_ permite escrever descrições em arquivos **markdown**.

**Arquivos markdown**: para construir o arquivo **markdown** para acompanhar a descrição do bloco de documentos, crie um novo arquivo chamado **core.md** com o seguinte caminho de arquivo: _models/marts/core/core.md_. Depois, copie e cole o trecho a seguir:

```
# the below are descriptions from fct_orders

{% docs gross_item_sales_amount %} same as extended_price {% enddocs %}
```

Este arquivo **markdown** possui um bloco _doc_ **({% docs gross_item_sales_amount %})** correspondente a uma coluna, na qual, sua descrição no arquivo _YAML_ **('{{ doc("gross_item_sales_amount") }}')** é este bloco _doc_ no **markdown**. 

O nome entre aspas na descrição do bloco de documento _YAML_ se conecta ao nome no bloco de documento de abertura em nosso arquivo **markdown**. Nesse caso, **'{{doc("gross_item_sales_amount") }}'** na descrição _YAML_ corresponde **{% docs gross_item_sales_amount %}** em nosso arquivo **markdown**. A partir daí, a descrição do texto em nosso arquivo **markdown** é o que é passado como descrição em nosso arquivo _YAML_.

Construir descrições com blocos de documentos em arquivos **markdown** oferece a capacidade de formatar suas descrições com **markdown** e é útil ao construir descrições longas. Além disso, se uma coluna existir em vários modelos com a mesma descrição, poderá reutilizar o mesmo bloco de documentos para essa coluna.

No arquivo **core.yml**, a coluna **item_discount_amount** contém a restrição de que os valores dessa coluna deve ser sempre menor ou igual a zero. Para averiguar podemos usar o **teste singular**. 

Para isso precisaremos de um **teste de dados personalizado**, então começaremos criando um novo arquivo chamado **fct_orders_negative_discount_amount.sql** com o seguinte caminho de arquivo: _tests/fct_orders_negative_discount_amount.sql_. Copie e cole o seguinte código no novo arquivo antes de salvar:

```
--If no discount is given it should be equal to zero 
--Otherwise it will be negative

select * from {{ ref('fct_orders') }}  
 where item_discount_amount > 0

```

Os **testes personalizados** são escritos como instruções ```select``` _SQL_ , onde um teste com falha retornará os registros com falha como saída. Isso significa que para que o teste seja aprovado, o resultado não retornará nenhuma linha e precisaremos escrever o teste com essa expectativa em mente. Dado que a nossa suposição sobre o **item_discount_amount** é que será sempre menor ou igual a zero, queremos então escrever o teste para procurar quando o valor é maior que zero.

Para executar este teste no arquivo _YAML_ **(core.yaml)**, o _DBT_ sabe quando executar através da combinação do arquivo que está no diretório **tests** (conforme definido em nosso arquivo **dbt_project.yml** ) e a instrução ```select``` usando a função ```ref``` para criar uma dependência com o modelo apropriado. Dessa forma, sempre que você usar o comando ```dbt test``` para testar esse modelo, esse teste será executado junto com todos os outros testes em nível de coluna.

Para fazer os teste, você pode inserir o seguinte comando na linha de comando: ```dbt test --select fct_orders```. Semelhante às execuções de nosso modelo, o código compilado que é passado ao Snowflake para cada teste é visível na seção **Details** de resultados do teste.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/b31d2789-c19b-4fc3-803a-87065ef2184e)

Sobre a documentação, o comando ```dbt docs generate``` irá dizer ao _DBT_ para criar os documentos. Após executar este comando, um ícone de livro ficará disponível na _IDE_ do _dbt_ ao lado do nome do seu **branch** (snowflake_workshop), na lateral esquerda superior. 

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/dd08bd6f-d944-4f79-9102-055725da846b)

O site de documentação será lançado em uma nova guia. Depois de carregar, entre na barra de pesquisa superior, digite **fct_orders** e clique no resultado superior para obter a documentação desse modelo.

Quando isso for carregado, você poderá ver nossa descrição em nível de modelo, as descrições em nível de coluna que escrevemos no arquivo _YAML_ e a descrição que veio dos blocos de documentos.

Agora reserve um momento para clicar no botão **commit** e enviar uma mensagem **tests and docs** para salvar suas alterações mais recentes.

Até este ponto, todo o trabalho que fizemos no _dbt Cloud IDE_ foi em nosso ambiente de desenvolvimento, com o código salvo em uma ramificação de desenvolvimento e os modelos que construímos criados em nosso esquema de desenvolvimento no Snowflake, conforme definido em nossa conexão com o ambiente desenvolvimento.

Agora que concluímos os testes e a documentação do nosso trabalho, estamos prontos para implantar nosso código do ambiente de desenvolvimento para o ambiente de produção e isso envolve duas etapas:

_1.0 Passando o código do nosso **branch de desenvolvimento** para o **branch de produção** (branch principal) em nosso repositório. Há um processo de revisão a ser realizado antes de mesclar o código com o **branch principal** de um repositório._
	
_2.0 Implantando código em nosso ambiente de produção. Depois que nosso código for mesclado ao **branch principal**, precisaremos executar o dbt em nosso ambiente de produção para construir todos os nossos modelos e executar todos os nossos testes. Felizmente, o fluxo do **partner connect** já criou nosso ambiente de implantação e trabalho para facilitar._
	
Para a primeira etapa, depois que todo o seu trabalho estiver confirmado, o botão de fluxo de trabalho do git aparecerá agora como **Merge to main**. Clique **Merge to main** e o processo de mesclagem será executado automaticamente. Quando estiver concluído, você deverá ver o botão lido **Create branche** o **branch** que você está vendo no momento se tornará **main**.

Para a segunda etapa, agora que todo o nosso trabalho de desenvolvimento foi mesclado ao **branch principal**, podemos construir nosso trabalho de implantação. Dado que nosso ambiente de produção e trabalho de produção foram criados automaticamente para nós por meio do **Partner Connect**, tudo o que precisamos fazer aqui é atualizar algumas configurações padrão para atender às nossas necessidades.

Clique na guia **Deploy** na barra superior e, em seguida, clique em **Environments**. Você deverá ver dois ambientes listados e deverá clicar no ambiente **Deployment**  para abri-lo e, em seguida, **Settings** no canto superior direito para modificá-lo.

Nosso trabalho de implantação será construído em nosso **PC_DBT_DBbanco** de dados e usará a função e o armazém padrão do **Partner Connect** para fazer isso. 

A seção de **credenciais de implantação** também usa as informações que foram criadas em nosso trabalho do **Partner Connect** para criar a conexão de credenciais. No entanto, ele está usando o mesmo esquema padrão que estamos usando como esquema para nosso **ambiente de desenvolvimento**. 

Vamos atualizar o esquema para criar um novo esquema especificamente para nosso **ambiente de produção**. Clique no botão **Edit**, no canto superior direito, para permitir que você modifique os valores dos campos existentes. Role para baixo até o campo **schema** na seção **Deployment Credentials** e atualize o nome do esquema para **production**. Certifique-se de clicar **Save**, no canto superior direito, depois de fazer a alteração.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/e02ccc22-7aec-409e-bdb2-950fed572254)

Ao atualizar o esquema de nosso **ambiente de produção** para **production**, ele garante que nosso trabalho de implantação para esse ambiente construirá nossos modelos _dbt_ no esquema **production** do banco de dados **PC_DBT_DB**, conforme definido na seção **Snowflake Connection**.

Agora vamos passar para o nosso trabalho de produção. Clique na guia **Deploy** na barra superior e, em seguida, clique em **Jobs**. Você deverá ver um arquivo **Partner Connect Trial Job**. Clique nele e selecione **Settings** para modificá-lo.

A seção **Ambiente** é o que conecta este trabalho ao ambiente em que queremos que ele seja executado. Este trabalho já está padronizado para usar o ambiente de implantação que acabamos de atualizar e o restante das configurações que podemos manter como estão a opção de gerar documentos, executar atualização de origem e adiar para um estado de execução anterior. 

Para os propósitos do nosso laboratório, vamos nos limitar apenas à geração de documentos. A seção **Comandos** é onde especificamos exatamente quais comandos queremos executar durante este trabalho.

Aqui você verá a seguinte estrutura: ```dbt seed```, ```dbt run```, ```dbt test```. Para este trabalho, iremos excluir ```dbt seed```, restando apenas os demais. 

Isso é feito, pois este projeto não possui uma semente, para projetos que possuem, as configurações deverão serem mantidas. Pois, queremos que nossa semente seja carregada primeiro, depois execute nossos modelos e, finalmente, teste-os. A ordem disso também é importante, considerando que precisamos que nossa semente seja criada antes de podermos executar nosso modelo incremental, e precisamos que nossos modelos sejam criados antes de podermos testá-los. 

Finalmente, temos a seção **Triggers**, onde temos uma série de opções diferentes para agendar nosso trabalho. Dado que nossos dados não são atualizados regularmente aqui e estamos executando esse trabalho manualmente por enquanto, também deixaremos esta seção de lado.

Nesta fase iremos alterar apenas o nome. Clique em **Edit**, no canto superior direito do trabalho, para permitir alterações. Em seguida, atualize o nome do trabalho para **Production Job** , na seção **General Settings**, para indicá-lo como nosso trabalho de implantação de produção. Depois de fazer isso, clique **Save** no topo da tela.

Agora vamos executar nosso trabalho. Clicar no nome do trabalho no caminho na parte superior da tela o levará de volta à página do histórico de execução do trabalho, onde você poderá clicar **Run now** para iniciar o trabalho.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/291bf63d-a262-4ad2-b397-4488eefd2d90)

Ao terminar você notará que o trabalho apresentou um erro, o que é estranho considerando que não havia não houve problemas quando executamos nossos trabalhos e testes anteriormente. Olhando mais de perto, podemos ver que há um problema na etapa de teste e queremos clicar nessa etapa para expandir os logs e ver o que está acontecendo.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/4ec19b8d-07f7-4cc4-876b-9832956d4af9)

Um dos testes dos nossos modelos de exemplo falhou! Isso foi um pouco inesperado, mas olhando todos os logs novamente podemos ver que os modelos que criamos foram construídos com sucesso no Snowflake e os testes para esses modelos foram aprovados. Neste ponto, podemos seguir em frente e corrigir o teste de exemplo com falha outro dia. Se preferir, podemos excluir os modelos de testes e rodar novamente os comandos para não houver erros.

Vamos ao Snowflake para confirmar se tudo foi construído conforme esperado em nosso esquema de produção. Atualize os objetos de banco de dados em sua conta Snowflake e você deverá ver o esquema **production** agora em nosso banco de dados padrão do **Partner Connect**. Se você clicar no esquema e tudo funcionar com sucesso, você poderá ver todos os modelos que desenvolvemos lá.

![image](https://github.com/Banco-Mercantil/analytics_code_dbt_snowflake/assets/88452990/5f7bb8ba-8b75-4c9a-b635-a99def83be66)
