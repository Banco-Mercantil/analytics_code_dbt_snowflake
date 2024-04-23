# PRIMEIROS PASSOS COM SNOWFLAKE E DBT CLOUD:
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

Vá para a guia "Worksheets", clique na opção de "+Worksheets" para adicionar uma nova planilha. Neste momento, clique em "Databases", você verá dois banco de dados e um deles se chama "snowflake sample data". Ao explandi-ló  você verá o schema "tpch sf1" e a partir daí tem-se todas as tabelas que possuem os dados brutos que serão transformados pelo projeto.

Para ter certeza, vamos exibir alguns resultados destas tabelas. Crie um "warehouse" de amostra e com a capacidade de tamanho pequena com a seguinte linha de comando:
  
```bash
create warehouse sample_warehouse with warehouse_size = xsmall
```

No quadro superior a direita, certifique de indicar a role "ACCOUNTADMIN" e o warehouse "SAMPLE_WAREHOUSE", em seguida, execute o seguinte trecho de comando: 
  
```bash
select *
from snowflake_sample_data.tpch_sf1.orders
limit 100;
```
Os resultados serão exibidos na sequência.

Com isso, é possível criar uma conta  DBT Labs através da partner connect. Pesquise por DBT em partner connect e selecione o item da pesquisa, no pop-up aberto, na seção "Optional Grant" informe o nome do banco de dados criado "SNOWFLAKE_SAMPLE_DATA". Siga com a conexão e ative-a.

Você será redirecionado para o site do DBT. Crie uma conta neste ambiente, clicando em "Complete Registration". Agora você deve ser redirecionado para sua conta DBT Cloud já com uma conexão com sua conta SNOWFLAKE.

Nesta etapa, para entrar na área de nuvem DBT, abra a IDE clicando no botão "Develop" no canto superior a esquerda. Quando o IDE terminar de carregar, clique no "Initialize dbt project", botão verde no canto superior esquerdo da tela. O processo de inicialização cria um projeto DBT na árvore de arquivos no lado esquerdo da tela com todos os arquivos e pastas principais do DBT necessários. 

Após o projeto criado, clique "Commit and sync", no canto superior esquerdo, para enviar/submeter os novos arquivos e pastas da etapa de inicialização. Um pop-up será aberto, informe uma mensagem de commit relevante para o trabalho: "initialize project". Na sequencia, clique em "Commit Changes" na janela pop-up.

_Commit criará uma nova versão com as edições acrescentadas ao projeto, gerando uma subversão do projeto. Push irá direcionar essa alteração da branch master para origin, ou seja, retirar do repositório local (Git) para o repositório remoto (GitHub)._
 
_Cada Commit dado, será criada uma nova versão, ao final do dia, dá-se o push para gerar uma cópia do projeto no GitHub (Origin)._

Ao confirmar, ele será salvo no repositório git gerenciado que foi criado durante a inscrição no Partner Connect. Este commit inicial é o único commit que será feito diretamente em nosso main branch (branch master/ramificação principal) e daqui em diante estaremos fazendo todo o nosso trabalho em um branch de desenvolvimento (merge no master). Isso nos permite manter nosso trabalho de desenvolvimento separado do código de produção.

A IDE possui alguns recursos chaves, ela é um editor de texto, um executor SQL e uma CLI com controle de versão git, tudo integrado em um único pacote. O fluxo de trabalho git no DBT Cloud permite controlar facilmente a versão de todo o seu trabalho com apenas alguns cliques. 

Clique em **+Create new file** para abrir o notebook da IDE. Nesta fase, um SQL Runner é aberto, uma linha de comando inferior na tela para executar comandos do DBT como ```dbt run``` e uma árvore de arquivos e controles git no canto superior a esquerda da tela.

![Imagem](https://quickstarts.snowflake.com/guide/accelerating_data_teams_with_snowflake_and_dbt_cloud_hands_on_lab/img/516d62e53cdc1ba1.png)

Agora é possível executar os primeiros modelos DBTs, os quais vem por padrão na pasta "models\example", para ilustrar como executar o dbt na linha de comando. Digite "dbt run" na linha de comando na parte inferior da tela e pressione "Enter" no teclado. Quando a barra de execução se expandir, você poderá ver os resultados da execução concluidos com sucesso.

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

