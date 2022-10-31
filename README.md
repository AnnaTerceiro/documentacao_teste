# Fluxo Diário Oficial de Alterações - FDO_ALT
# Sumário
#
## 1 - Resumo do projeto
Captura e estrutura dados de alterações de registro e pós-registro publicadas no Diário Oficial.

Cada ementa poderá tratar de um ou mais tipos específicos de alteração. Há uma tabela SQL que relaciona todos os tipos conhecidos (dTipAlt).

É criado uma Extração, uma Transformação para os dados e o Load para o SQL, e após o processo, será visualizado pelo cliente. Todo o fluxo diário é reunido em uma **Dag** implementada no Airflow.
#
## 2 - Contextualização 
Uma publicação típica é composta de parágrafos de texto não estruturado aos quais chamamos de ementas. Cada ementa irá tratar sobre um ou mais registros, um ou mais processos e uma ou mais empresas. Também poderá ser sobre retificação de uma ementa publicada anteriormente.

Cada ementa poderá tratar de um ou mais tipos específicos de alteração. Toda ementa deve ter um ou mais tipos associados. Caso não haja, deverá ser gerado um aviso a ser tratado no log (arquivo de erros não críticos que precisam ser tratados pós processamento da DAG) e um aviso de warning (mensagem de texto que irá alertar o responsável por qualidade de dados).

A captura dos dados deve acontecer em modelo de ‘webscrapping’ com Python e os dados são extraídos necessariamente do site do Diário Oficial da União.
#
## 3 - Desenho da arquitetura
#
## 4 - Descrição da arquitetura
#
## 5 - Dicionário dos dados
### $\color{white} \colorbox{Red} {dbo.xDouAlt} $
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

### $\color{white} \colorbox{Red} {dbo.xDouAlt CNPJ} $
| Nome  | Tipo |
|-------|------|
| IdBRAlt  | int |
| CNPJ  | varchar(50) |
| DtAtu  | datetime |

### $\color{white} \colorbox{Red} {dbo.xDouAlt Processo} $
| Nome  | Tipo |
|-------|------|
| IdBRAlt  | int |
| Processo_Mapa | varchar(25) |
| DtAtu | datetime |
| iPrincipal | int |

### $\color{white} \colorbox{Red} {dbo.xDouAlt Registro} $
| Nome  | Tipo |
|-------|------|
| IdBRAlt  | int |
| Registro | varchar(10) |
| DtAtu | datetime |
| RET | int |
#
### 6 - Visão relações tabelas fato e dimensão
![Documentação #1]([https://github.com/AnnaTerceiro/documentacao_teste/issues/1#issue-1430506832](https://user-images.githubusercontent.com/47015493/199111801-0ed73f99-9be3-4bd7-9943-e26a1f4e041a.PNG)
