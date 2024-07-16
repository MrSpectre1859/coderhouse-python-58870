# Projeto de Pipeline de Dados: Informações Geográficas do Brasil

## Introdução

Este projeto consiste na construção de um pipeline de dados que realiza a extração, tratamento, e armazenamento de dados geográficos do Brasil, utilizando APIs públicas. O objetivo é permitir que dados brutos sejam coletados, processados, analisados e disponibilizados para uso em diferentes áreas de negócios.

Escolhi trabalhar com dados de endereços do Brasil porque são informações amplamente utilizadas em diversas áreas de empresas, como logística, marketing e planejamento. A capacidade de extrair e tratar esses dados de maneira eficiente pode beneficiar muitas empresas e instituições que necessitam de dados geograficamente localizados e organizados, e que por vezes é difícil de se obter de forma estruturada.

## Estrutura do Projeto

- `main.ipynb`: Script principal contendo todo o código do projeto.
- `README.md`: Documentação do projeto.
- `requirements.txt`: Arquivo contendo todas as dependências necessárias para executar o projeto.
- `ceps.csv`: Arquivo csv com uma única coluna a ser editada pelo usuário e que deverá conter os ceps a serem procurados
- `dados_extraidos_ceps.xlsx`: Arquivo excel contendo as informações extraída em cada tabela dentro do notebook, para maior versatilidade em ambientes corporativos


## Configuração do Ambiente

1. Clone o repositório para a sua máquina local:
    ```bash
    git clone https://github.com/MrSpectre1859/coderhouse-python-58870.git
    cd <LOCAL_DO_SEU_REPOSITORIO>
    ```

2. Crie e ative um ambiente virtual:
    ```bash
    cd /caminho/para/seu/projeto
    python -m venv venv
    source venv\Scripts\activate 
    ```

3. Instale as dependências:
    ```bash
    pip install -r requirements.txt
    ```

4. Abra o Jupyter Notebook:
    ```bash
    jupyter notebook
    ```

5. Abra o 'projeto_final_coder.ipynb'

6. No Jupyter Notebook, vá em "Kernel" > "Change Kernel" > "Python (venv)"

7. Execute todas as células

## Principais dependências

- pandas==2.1.2
- requests==2.31.0
- plyer==2.1.0
- SQLAlchemy==2.0.31
- jupyter==1.0.0

## Processo de Extração de Dados

### API de CEPs

Os dados foram obtidos utilizando a BrasilAPI, que fornece endpoints públicos e gratuitos para acessar diversas informações geográficas e de endereços do Brasil. Escolhemos a BrasilAPI devido à sua simplicidade de uso, confiabilidade apresentada e abrangência de dados.

Os dados são extraídos de APIs públicas disponibilizadas pelo BrasilAPI:

- Endereços por CEP: https://brasilapi.com.br/api/cep/v1/{cep}
- Municípios por Estado: https://brasilapi.com.br/api/ibge/municipios/v1/{state}?providers=dados-abertos-br,gov,wikipedia
- Estados do Brasil: https://brasilapi.com.br/api/ibge/uf/v1/{state}

### Dicionário de Dataframes

#### df_table_1

* cep: Código de Endereçamento Postal,
* estado: noem do estado,
* cidade: nome da cidade,
* bairro: nome do bairro ,
* rua: nome da rua

#### df_table_2

* estado : nome do estado,
* nome: Nome do município,
* codigo_ibge: Código IBGE do município

### df_table_3

* id:  identificador único do estado,
* sigla: Sigla do estado,
* nome: Nome do estado,
* id_regiao: identificador único da região,
* sigla_regiao: sigla da região,
* nome_regiao: nome da região,

### Exemplo de extração de dados

Abaixo está um exemplo de como extraímos dados de endereços utilizando CEPs:

```python
import requests

ceps = ['27511630', '27323330', '27510031']
data_table_1 = []

for cep in ceps:
    url = f'https://brasilapi.com.br/api/cep/v1/{cep}'
    response = requests.get(url)
    if response.status_code == 200:
        data_table_1.append(response.json())
