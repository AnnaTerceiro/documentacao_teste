# Fluxo Diário Oficial de Alterações - FDO_ALT
# Sumário
## $\color{black} \colorbox{Yellow} {1 - Resumo do projeto} $
Captura e estrutura dados de alterações de registro e pós-registro publicadas no Diário Oficial.

Cada ementa poderá tratar de um ou mais tipos específicos de alteração. Há uma tabela SQL que relaciona todos os tipos conhecidos (dTipAlt).

É criado uma Extração, uma Transformação para os dados e o Load para o SQL, e após o processo, será visualizado pelo cliente. Todo o fluxo diário é reunido em uma **Dag** implementada no Airflow.
#
## $\color{black} \colorbox{Yellow} {2 - Contextualização} $
Uma publicação típica é composta de parágrafos de texto não estruturado aos quais chamamos de ementas. Cada ementa irá tratar sobre um ou mais registros, um ou mais processos e uma ou mais empresas. Também poderá ser sobre retificação de uma ementa publicada anteriormente.

Cada ementa poderá tratar de um ou mais tipos específicos de alteração. Toda ementa deve ter um ou mais tipos associados. Caso não haja, deverá ser gerado um aviso a ser tratado no log (arquivo de erros não críticos que precisam ser tratados pós processamento da DAG) e um aviso de warning (mensagem de texto que irá alertar o responsável por qualidade de dados).

A captura dos dados deve acontecer em modelo de ‘webscrapping’ com Python e os dados são extraídos necessariamente do site do Diário Oficial da União.
#
## $\color{black} \colorbox{Yellow} {3 - Desenho da arquitetura} $
#
## $\color{black} \colorbox{Yellow} {4 - Descrição da arquitetura} $
#
## $\color{black} \colorbox{Yellow} {5 - Dicionário dos dados} $
### $\color{white} \colorbox{Green} {dbo.xDouAlt} $
| Nome  | Tipo |
|-------|------|
| IdBRAlt  | int |
| Ato | int |
| DAto | date |
| SeqAto | int |
| DDou | datetime |
| Ementa | varchar(MAX) |
| DescAto | varchar(100) |
| DtAtu | datetime |
| IdTipAlt | int |
| LinkGov | varchar(MAX) |

### $\color{white} \colorbox{Green} {dbo.xDouAlt CNPJ} $
| Nome  | Tipo |
|-------|------|
| IdBRAlt  | int |
| CNPJ  | varchar(50) |
| DtAtu  | datetime |

### $\color{white} \colorbox{Green} {dbo.xDouAlt Processo} $
| Nome  | Tipo |
|-------|------|
| IdBRAlt  | int |
| Processo_Mapa | varchar(25) |
| DtAtu | datetime |
| iPrincipal | int |

### $\color{white} \colorbox{Green} {dbo.xDouAlt Registro} $
| Nome  | Tipo |
|-------|------|
| IdBRAlt  | int |
| Registro | varchar(10) |
| DtAtu | datetime |
| RET | int |
#
### $\color{black} \colorbox{Yellow} {6 - Visão relações tabelas fato e dimensão} $
![Documentação #1](https://user-images.githubusercontent.com/47015493/199111801-0ed73f99-9be3-4bd7-9943-e26a1f4e041a.PNG)
#
## $\color{black} \colorbox{Yellow} {7 - Regras de negócio} $
RN01: Identificar se houve ou não houve publicação de alterações no dia. Se não houver, pula as etapas de transformação e carga e envia e-mail de notificação

RN02: Se houver publicação - Captura todas as ementas. Para cada ementa:

1. Se ementa for de retificação:
- Buscar a qual ementa pertence a retificação utilizando a tabela

2. Se ementa não for de retificação:
- Identificar o tipo de ementa (dTipAlt)
- Estruturar os dados da Ementa
- Identificar o(s) número(s) de registro citados na ementa e montar data frame
- Identificar o(s) número(s) de processo citados na ementa e montar data frame
- Identificar o(s) número(s) de CNPJ citados na ementa e montar data frame

RN03: Carregar dados preparados no TRANSFORM para o SQL
