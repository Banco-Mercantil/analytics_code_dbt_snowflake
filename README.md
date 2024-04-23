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

![Screenshot of a comment on a GitHub issue showing an image, added in the Markdown, of an Octocat smiling and raising a tentacle.](https://myoctocat.com/assets/images/base-octocat.svg)





do VS Code aberta, acesse o terminal pelo atalho *Ctrl + '*. 
Acesse o caminho no qual deseja configurar seu primeiro projeto:
```bash
PS: ...> cd K:\
  PS: K:\>
PS: K:\> cd GEC\2024\'04. Dados'\'0_Snowflake' 
  PS K:\GEC\2024\04. Dados\0_Snowflake>
```

Dentro da pasta na qual o projeto irá ser salvo, executar o comando:	
```bash
PS K:\GEC\2024\04. Dados\0_Snowflake> dbt init
  Running with dbt=1.5.0
  Enter a name for yout project (letters, dgits, underscore): NOME_DO_NOVO_PROJETO
    Precione "Enter"
```

Informe os parâmetros da conexão com o banco de dados seguindo as instruções exibidas no terminal:
```bash
Which database would you like to use?
[1]snowflake
Enter a number: 1
account (https://<this_value>.snowflakecomputing.com): ffa72186.us-east-1		
user (dev username): xxxxxxx@MERCANTIL.COM.BR
[1] password	
[2] keypair
[3] sso
Desired authentication type option (enter a number): 3
authenticator ('externalbrowser' or a valid Okta URL) [externalbrowser]: externalbrowser
role (dev role): SNFLK_AD_GERENCIA_EXCELENCIA_COMERCIAL
warehouse (warehouse name): WH_DEV
database (default database that dbt will build objects in): SDX_EXCELENCIA_COMERCIAL
schema (default schema that dbt will build objects in):  EFET_CAMPANHAS__INCENVITO_REDE
threads (1 or more) [1]: 16
  PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO>
```

Nesta fase, o projeto foi criado junto com seus respectivos arquivos como: .vscode, analyses, dbt_project.yml. Para abri-ló basta digitar o comando seguinte:
```bash
PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO> code .
```
 
Acesse o terminal do novo VS Code aberto através do comando anterior *Ctrl '*:
```bash
PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO>
```

Teste a conexão criada entre o projeto e o banco de dados executando a linha de comando abaixo:
```bash
PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO> dbt debug
  Registered adapter: snowflake=1.7.3
  Connection test: [OK connection ok]
  All checks passed!
  PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO>
```

## 🚀 Inicializando o projeto DBT em ambiente virtual
Com a ferramenta do VS Code aberta, acesse o terminal pelo atalho *Ctrl'*. 
Acesse o caminho no qual deseja configurar seu primeiro projeto:
```bash
PS: ...> cd K:\
  PS: K:\>
PS: K:\> cd GEC\2024\'04. Dados'\'0_Snowflake' 
  PS K:\GEC\2024\04. Dados\0_Snowflake>
```

Crie um ambiente virtual:
```bash
PS K:\GEC\2024\04. Dados\0_Snowflake> python -m venv dbt_venv
```

Ative o ambiente virtual criado:
*Obs*: Comando para sistema Windows, este muda no sistema Linux.
```bash
PS K:\GEC\2024\04. Dados\0_Snowflake> .\dbt_venv\Scripts\Activate.ps1
  (dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake>
```

Instale o adapter que estiver utilizando:
```bash
(dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake> pip install dbt_snowflake
  [notice] To update, run: python.exe -m pip install --upgrade pip
  (dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake>
```

Antes de iniciar um novo projeto dbt com o ambiente virtual, é necessário criar uma pasta *.dbt* no diretório do seu usuário. Este arquivo manterá o profile.yaml e as credenciais do usuário são armazenadas:
```bash
(dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake> mkdir $home\.dbt
```

Para iniciar o novo projeto, execute:	
```bash
(dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake> dbt init		
  Running with dbt=1.5.0
  Enter a name for yout project (letters, dgits, underscore): NOME_DO_NOVO_PROJETO
    Precione "Enter"
```

Informe os parâmetros da conexão com o banco de dados seguindo as instruções exibidas no terminal:
```bash
Which database would you like to use?
[1]snowflake
Enter a number: 1
account (https://<this_value>.snowflakecomputing.com): ffa72186.us-east-1		
user (dev username): xxxxxxx@MERCANTIL.COM.BR
[1] password	
[2] keypair
[3] sso
Desired authentication type option (enter a number): 3
authenticator ('externalbrowser' or a valid Okta URL) [externalbrowser]: externalbrowser
role (dev role): SNFLK_AD_GERENCIA_EXCELENCIA_COMERCIAL
warehouse (warehouse name): WH_DEV
database (default database that dbt will build objects in): SDX_EXCELENCIA_COMERCIAL
schema (default schema that dbt will build objects in):  EFET_CAMPANHAS__INCENVITO_REDE
threads (1 or more) [1]: 16
  (dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO>
```

Nesta fase, o projeto foi criado junto com seus respectivos arquivos como: .vscode, analyses, dbt_project.yml. Para abri-ló basta digitar o comando seguinte:
```bash
(dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO> code .
```

Acesse o terminal do novo VS Code aberto através do comando anterior *Ctrl '* e ative o ambiente virtual:
```bash
PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO> K:\GEC\2024\04. Dados\0_Snowflake\dbt_venv\Scripts\Activate.ps1
  (dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO> 
```

Teste a conexão criada entre o projeto e o banco de dados executando a linha de comando abaixo:
```bash
(dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO> dbt debug
  Registered adapter: snowflake=1.7.3
  Connection test: [OK connection ok]
  All checks passed!
  (dbt_venv) PS K:\GEC\2024\04. Dados\0_Snowflake\NOME_DO_NOVO_PROJETO>
```

Note que na pasta *.dbt*, criada no diretório do usuário, agora possui um arquivo profiles.yaml. Este arquivo armazena os detalhes da conexão informada anteriormente na inicialização do dbt.

