# churn-analysis-delivery-app
Este projeto aplica técnicas de análise de dados e machine learning para prever e entender o churn de usuários em um aplicativo de delivery.

A análise foi conduzida utilizando SQL para tratamento inicial dos dados, e Excel para vizualização, consulta e manipulação dos dados de usuários, pedidos e interações. foram aplicadas técnicas de análise exploratória, estatística descritiva e medidas de associação, como o Information Value (IV), para identificar fatores de risco e segmentar clientes conforme a probabilidade de Churn.

Todo o processo foi documentado em um artigo no Medium, onde explico detalhadamente a metodologia, insights e resultados obtidos.
O arquivo com as planilhas estão em ProjetoChurnDelivery

ESSES FORAM OS CODIGOS UTILIZADOS NO SQL
--projeto churn tabela 1
SELECT*
FROM
db_churn.fato_churn
LIMIT 5;

SELECT*
FROM
db_churn.dim_clientes
LIMIT 5;

-- VERIFICAÇÃO DE VALORES NULOS - TABELA dim_clientes
SELECT 
    'dim_clientes' AS tabela,
    COUNT(CASE WHEN "ClientId" IS NULL THEN 1 END) AS nulos_ClientId,
    COUNT(CASE WHEN "DataExtracao" IS NULL THEN 1 END) AS nulos_DataExtracao,
    COUNT(CASE WHEN "Score_Credito" IS NULL THEN 1 END) AS nulos_Score_Credito,
    COUNT(CASE WHEN "Estado" IS NULL THEN 1 END) AS nulos_Estado,
    COUNT(CASE WHEN "Gênero" IS NULL THEN 1 END) AS nulos_Genero,
    COUNT(CASE WHEN "Idade" IS NULL THEN 1 END) AS nulos_Idade,
    COUNT(CASE WHEN "Tempo_Cliente" IS NULL THEN 1 END) AS nulos_Tempo_Cliente,
    COUNT(CASE WHEN "Limite_Credito_Mercado" IS NULL THEN 1 END) AS nulos_Limite_Credito,
    COUNT(CASE WHEN "Qte_Categorias" IS NULL THEN 1 END) AS nulos_Qte_Categorias,
    COUNT(CASE WHEN "Usa_Cartao_Credito" IS NULL THEN 1 END) AS nulos_Usa_Cartao,
    COUNT(CASE WHEN "Programa_Fidelidade" IS NULL THEN 1 END) AS nulos_Programa_Fidelidade,
    COUNT(CASE WHEN "Sum_Pedidos_Acumulados" IS NULL THEN 1 END) AS nulos_Pedidos_Acumulados,
    COUNT(*) AS total_linhas
FROM db_churn.dim_clientes

UNION ALL

-- VERIFICAÇÃO DE VALORES NULOS - TABELA fato_churn
SELECT 
    'fato_churn' AS tabela,
    COUNT(CASE WHEN "ClientId" IS NULL THEN 1 END) AS nulos_ClientId,
    COUNT(CASE WHEN "DataUltimaTransacao" IS NULL THEN 1 END) AS nulos_DataUltimaTransacao,
    NULL AS nulos_Score_Credito,
    NULL AS nulos_Estado,
    NULL AS nulos_Genero,
    NULL AS nulos_Idade,
    NULL AS nulos_Tempo_Cliente,
    NULL AS nulos_Limite_Credito,
    NULL AS nulos_Qte_Categorias,
    NULL AS nulos_Usa_Cartao,
    NULL AS nulos_Programa_Fidelidade,
    NULL AS nulos_Pedidos_Acumulados,
    COUNT(*) AS total_linhas
FROM db_churn.fato_churn;

-- UNIÃO DAS TABELAS

SELECT DISTINCT ON (d."ClientId")
       d.*, 
       f."DataUltimaTransacao"
FROM db_churn.dim_clientes d
JOIN db_churn.fato_churn f 
     ON d."ClientId" = f."ClientId"
ORDER BY d."ClientId", f."DataUltimaTransacao" DESC
LIMIT 10;




