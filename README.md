# TechnicalTest
## Arkano Technical test 
Solución que utiliza los siguientes recursos de Azure para la carga de información proveniente de un archivo plano formato CVS
  -Azure Blob Storage para el almacenamiento de la información
  -Azure Synapse Analitics para el proceso de carga a traves de un flujo de datos con el uso de "copy data"
  -SQL Server para la carga de tablas y posterior uso de ellas
  -Power Bi para visualización y reporte

La arquitectura propuesta seria la siguiente, de acuerdo a las recomendaciones de Microsoft:
![Proposed_Architecture]![Proposed_Architecture](https://github.com/aldocarloscm/TechnicalTest/assets/125507525/97da6e5f-b9b1-4e8b-92f6-3e3dc74b4cb0)

  

### Query que realiza la extracción de información de forma cuatrimestral en la base de datos

SELECT product,[Sub-product],Q1,Q2,Q3,Q4 from (
SELECT product,[Sub-product],[Complaint ID],
CASE
    WHEN MONTH([Date received]) BETWEEN 1 AND 3 THEN 'Q1'
    WHEN MONTH([Date received]) BETWEEN 4 AND 6 THEN 'Q2'
    WHEN MONTH([Date received]) BETWEEN 7 AND 9 THEN 'Q3'
    WHEN MONTH([Date received]) BETWEEN 10 AND 12 THEN 'Q4'
  END AS custom_quarter
  FROM [dbo].[complaints]
  where year([Date received]) = '2023'
 ) a
PIVOT (
    COUNT([Complaint ID])
    FOR custom_quarter IN (Q1,Q2,Q3,Q4)
) AS PIVOTTABLE****

