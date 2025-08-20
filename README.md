# ChallengeAluraTelecomX

# Relatório

Este relatório descreve o passo a passo das etapas de extração e transformação realizadas nos dados para posteriormente ser feita uma análise exploratória para descobrir possíveis razões de Churn da empresa Telecom X e supor possíveis soluções para o problema.

### Extração

  * Importação das bibliotecas `pandas`, `matplotlib.pyplot` e `seaborn`.
  * Definição do URL para o arquivo JSON (`TelecomX_Data.json`).
  * Leitura dos dados do URL utilizando `pd.read_json()` e armazenamento no DataFrame `dados`.
  * Exibição das primeiras linhas do DataFrame `dados` para inspeção inicial.

### Transformação

 * Normalização das colunas aninhadas (`customer`, `phone`, `internet`, `account`) utilizando `pd.json_normalize()`.
 * Criação de DataFrames separados para cada coluna normalizada: `customer_normalized`, `phone_normalized`, `internet_normalized`, `account_normalized`.
 * Remoção das colunas aninhadas originais do DataFrame `dados` para criar `dados_flat`.
 * Concatenação horizontal (`axis=1`) do `dados_flat` com os DataFrames normalizados para criar o DataFrame `dados` unificado.
 * Configuração da opção de exibição do pandas para mostrar todas as colunas (`pd.set_option('display.max_columns', None)`).
 * Exibição do DataFrame `dados` completo após a normalização e concatenação.

 * Criação de uma nova coluna chamada `Contas_diarias`.
 * Cálculo dos valores para a coluna `Contas_diarias` dividindo os valores da coluna `Charges.Monthly` por 30 e arredondando para duas casas decimais.
 * Exibição do DataFrame `dados` com a nova coluna `Contas_diarias`.

 * Cálculo da contagem e percentual de valores nulos (`isnull().sum()`) para cada coluna do DataFrame `dados`.
 * Cálculo da contagem e percentual de valores vazios (`astype(str).apply(lambda x: x.str.strip() == '').sum()`) para cada coluna do DataFrame `dados`.
 * Criação de um DataFrame resumo (`summary_df`) com as contagens e percentuais de nulos e vazios.
 * Filtragem do DataFrame resumo (`summary_df_filtered`) para mostrar apenas as colunas que possuem contagem de nulos ou vazios maior que zero.
 * Exibição do DataFrame `summary_df_filtered`.

 * Identificação das linhas no DataFrame `dados` onde a coluna 'Churn' contém valores vazios ('').
 * Exibição das linhas identificadas com valores vazios na coluna 'Churn'.

 * Identificação das linhas no DataFrame `dados` onde a coluna 'Charges.Total' contém valores vazios ('').
 * Exibição das linhas identificadas com valores vazios na coluna 'Charges.Total'.
 * Identificação de linhas duplicadas no DataFrame `dados` utilizando o método `duplicated(keep=False)`.
 * Exibição das linhas duplicadas encontradas ou uma mensagem indicando que não há duplicatas.

 * Substituição global dos valores "Yes" por 1 e "No" por 0 em todo o DataFrame `dados` utilizando o método `replace()`.
 * Exibição das primeiras linhas do DataFrame `dados` após a substituição.

Este relatório resume as principais ações realizadas nas etapas de extração e transformação dos seus dados.

## Análise da Seção "Carga e Análise" - Possíveis Causas de Churn

### Distribuição de Churn

O gráfico de pizza da **"Distribuição de Churn"** 

<img width="425" height="439" alt="image" src="https://github.com/user-attachments/assets/6e33ce52-dbe3-471d-9e88-304bcc08032f" />

Mostrou que aproximadamente 26.5% dos clientes na base de dados cancelaram o serviço.

### Relação entre SeniorCitizen e Churn

O gráfico de barras da **"Relação entre SeniorCitizen e Churn"** 

<img width="527" height="407" alt="image" src="https://github.com/user-attachments/assets/c45cb8ba-c0e2-4546-9d8c-dbc38faacf3e" />

comparou o churn entre clientes com menos de 65 anos e mais de 65 anos. Observamos que:

* A maioria dos clientes na base tem menos de 65 anos.
* Em termos absolutos, há mais clientes com menos de 65 anos que cancelaram o serviço.
* Porém o grupo de mais de 65 anos porporcionalmente tem uma maior desistência.

### Relação entre Partner e Churn

O seguinte explorou a relação entre ter um parceiro e o churn. 

<img width="528" height="411" alt="image" src="https://github.com/user-attachments/assets/e2756e76-5afd-48db-8640-d6d2a862a626" />

Os resultados indicaram que:

*   Clientes sem parceiro ("Não tem parceiro") parecem ter uma contagem maior de churn do que aqueles com parceiro ("Tem parceiro").
*   Isso sugere que clientes com parceiros podem ter uma maior lealdade ou satisfação com o serviço, ou talvez os planos e ofertas sejam mais atraentes para casais ou famílias.

### Relação entre Tipo de Contrato e Churn

Os gráficos de barras relacionando os tipos de contrato ("Month-to-month", "One year", "Two year") com o Churn revelaram uma forte associação:

## Mensal

<img width="398" height="296" alt="image" src="https://github.com/user-attachments/assets/f5c68119-4522-45e2-bc42-94b99f9dc05d" />

## Anual

<img width="412" height="294" alt="image" src="https://github.com/user-attachments/assets/c7e6b4b0-2c05-47cc-addf-8f87cb8c9800" />

## Bienal

<img width="412" height="294" alt="image" src="https://github.com/user-attachments/assets/8a9d4a4a-a5c7-416e-83d2-b60316f28369" />

* Clientes com contratos **"Month-to-month"** (mês a mês) apresentaram a maior contagem de churn. Isso faz sentido, pois contratos de curto prazo oferecem mais flexibilidade para o cliente cancelar a qualquer momento.
* Clientes com contratos de **"One year"** (um ano) tiveram uma contagem de churn significativamente menor em comparação com os de contrato mensal.
* Clientes com contratos de **"Two year"** (dois anos) apresentaram a menor contagem de churn. Contratos de longo prazo geralmente indicam maior compromisso e satisfação do cliente.

A duração do contrato parece ser um fator crucial na retenção de clientes.

### Relação entre Método de Pagamento e Churn

O gráfico de barras examinou como os diferentes métodos de pagamento se relacionam com o churn. Observou-se que:

<img width="752" height="373" alt="image" src="https://github.com/user-attachments/assets/1804a738-5ab9-45d5-93eb-1e7c862bb40f" />

* O método de pagamento **"Electronic check"** (cheque eletrônico) teve a maior contagem de churn em comparação com outros métodos como "Mailed check", "Credit card (automatic)" e "Bank transfer (automatic)".
* Isso pode indicar que há algum problema ou insatisfação associada ao uso de cheque eletrônico, ou talvez os clientes que escolhem este método tenham outras características que os tornam mais propensos ao churn.

### Relação entre Serviço de Telefone e Churn

O gráfico de barras da comparou o churn entre clientes que possuem serviço de telefone e aqueles que não possuem. A análise visual sugere que:

<img width="455" height="357" alt="image" src="https://github.com/user-attachments/assets/15f87516-925d-4cfe-9d53-7793503f0df5" />


* A maioria dos clientes possui serviço de telefone.
* Em termos de contagem, há mais churn entre os clientes que **têm** serviço de telefone. No entanto, a diferença na taxa de churn entre os dois grupos (quem tem vs quem não tem) seria uma análise mais precisa.

### Relação entre Múltiplas Linhas e Churn

O gráfico de barras explorou a relação entre ter múltiplas linhas telefônicas e o churn. 

<img width="457" height="356" alt="image" src="https://github.com/user-attachments/assets/74c49df9-b00c-4537-bc53-8c9ddb656bf4" />

Os resultados indicaram:

* Clientes com **múltiplas linhas** ("1") parecem ter uma contagem de churn ligeiramente maior do que aqueles com apenas uma linha ("0").
* Os clientes sem serviço de telefone ("No phone service") naturalmente não têm múltiplas linhas e sua contagem de churn é menor, consistente com o gráfico anterior.

* * *

**Resumo das Possíveis Razões para Churn:**

Com base nesta análise inicial, os fatores que parecem ter a relação mais forte com o churn são:

1.  **Tipo de Contrato**: Clientes com contratos mês a mês são significativamente mais propensos a cancelar.
2.  **Método de Pagamento**: Clientes que usam cheque eletrônico parecem ter uma maior probabilidade de churn.
3.  **Status de Parceiro**: Clientes sem parceiro podem ser mais propensos ao churn.

## Possíveis Soluções para Reduzir o Churn

Com base nos fatores identificados que parecem influenciar o churn, aqui estão algumas possíveis soluções, divididas em ações de curto e longo prazo:

### Curto Prazo

*   **Campanhas de Retenção para Clientes com Contrato Mensal:** Oferecer promoções ou benefícios exclusivos para clientes com contratos mês a mês que estão em risco de churn. Isso pode incluir descontos temporários, upgrade de serviço ou ofertas especiais para incentivá-los a permanecer.
*   **Comunicação Proativa:** Entrar em contato com clientes que utilizam cheque eletrônico para entender se há algum problema ou insatisfação específica com esse método de pagamento e oferecer alternativas ou suporte.
*   **Ofertas para Clientes Sem Parceiro:** Desenvolver ofertas ou pacotes que sejam atraentes para clientes individuais, talvez focando em benefícios que não sejam necessariamente relacionados a múltiplos usuários.
*   **Melhoria no Processo de Pagamento com Cheque Eletrônico:** Investigar possíveis pontos de atrito ou problemas técnicos no processo de pagamento com cheque eletrônico e implementar melhorias rápidas.

### Longo Prazo

*   **Reestruturação de Planos e Contratos:** Avaliar a estrutura dos planos de contrato, especialmente os de curto prazo (mês a mês), para identificar se eles são menos atraentes ou se há formas de incentivar a migração para contratos mais longos com benefícios adicionais.
*   **Análise Aprofundada do Método de Pagamento:** Realizar uma investigação mais profunda sobre por que o cheque eletrônico está associado a um maior churn. Isso pode envolver pesquisas com clientes, análise de dados transacionais e feedback para identificar causas raiz e implementar soluções estruturais.
*   **Programas de Fidelidade:** Desenvolver programas de fidelidade robustos que recompensem clientes de longo prazo e aqueles com contratos mais duradouros, aumentando o valor percebido de permanecer com a empresa.
*   **Personalização de Ofertas:** Utilizar os dados do cliente para personalizar ofertas e comunicações, tornando-as mais relevantes para as necessidades e características de diferentes segmentos de clientes (incluindo status de parceiro e idade).
*   **Acompanhamento Contínuo e Modelagem Preditiva:** Continuar monitorando as métricas de churn, refinar as análises e considerar a construção de modelos preditivos para identificar clientes em risco de churn com antecedência e permitir ações de retenção mais direcionadas e eficazes.

