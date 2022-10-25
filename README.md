# Fluxo Diário Oficial de Alterações - FDO_ALT
# Sumário
#
## 1 - Resumo do projeto
Captura e estrutura dados de alterações de registro e pós-registro publicadas no Diário Oficial.

Cada ementa poderá tratar de um ou mais tipos específicos de alteração. Há uma tabela SQL que relaciona todos os tipos conhecidos (dTipAlt).

É criado uma Extração, uma Transformação para os dados e o Load para o SQL, e após o processo, será visualizado pelo cliente. Todo o fluxo diário é reunido em uma **Dag** implementada no Airflow
