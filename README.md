## Desafio de projeto da DIO [Modelando um Dashboard de E-commerce com Power BI Utilizando Fórmulas DAX](https://web.dio.me/lab/modelagem-e-transformacao-de-dados-com-dax-com-power-bi/learning/b1c8ea6c-0b78-4d47-a873-9d911d79660b?back=/track/engenharia-dados-python)
#### [Guia disponibilizado](https://academiapme-my.sharepoint.com/:w:/g/personal/renato_dio_me/EW76WjPAA8RGgC3i44ofFq4BBiWzM-CN5S312YwOQCIwBA?e=7A6KfG)

---


# Introdução
Este projeto implementa um modelo em estrela no Power BI utilizando os dados da tabela Financials fornecida pelo PowerBI e uma tabela datas criada com código DAX.

O que é o modelo estrela?
O modelo estrela é uma forma de organizar tabelas em um banco de dados de forma que haja uma tabela central (fato) que contenha dados quantitativos e várias tabelas dimensionais ao seu redor que descrevem os dados da tabela fato. Ele é amplamente utilizada em projetos de Business Intelligence (BI) para facilitar a análise de grandes volumes de dados.

# Estrutura do Modelo
O modelo em estrela criado possui as seguintes tabelas e relacionamentos:

## Tabelas Fato:
F_Vendas: Esta tabela contém os dados de vendas, como:

Sales: Valor das vendas.
Country: País da venda.
Date: Data da venda.
Discount Band: Faixa de desconto.
ID_Produto: Relaciona-se com a tabela D_Produtos
Product: Produto vendido.
Profit: Lucro das vendas.
Sale Price: Preço de venda.
Units Sold: Unidades vendidas.


Tabelas Dimensionais:

D_Produtos: Esta tabela contém as informações relacionadas aos produtos:

ID_Produto: Identificador único do produto.
Product: Nome do produto.
Métricas calculadas, como:
Cotagem
Valor máximo de Venda
Valor mínimo de Venda
Médias do valor de vendas


D_Calendário: Criada com DAX:

````
D_Calendário = 
CALENDAR(
    MIN(financials_origem[Date]), 
    MAX(financials_origem[Date])
)
````


D_Descontos: Tabela auxiliar que contém:

Discount Band: Faixa de desconto.
Discounts: descotos
ID_Produto: Relaciona-se com a tabela D_Produtos.


D_Detalhes: Esta tabela contém informações detalhadas sobre as vendas, como:

COGS: Custo dos bens vendidos.
Country: País.
ID_Detalhes: Identificador de detalhes.
ID_Produto: Identificador do produto (relaciona-se com D_Produtos).
Month Name e Year: Relacionado ao tempo.

---

Processo de Criação
1. Importação de Dados
Os dados foram importados da tabela Financials_origem e, a partir dela, foram derivadas tabelas fato e dimensionais.
2. Criação da Tabela de Calendário (D_Calendário). A tabela de calendário foi criada utilizando a função DAX CALENDAR() no Power BI:
3. Relacionamentos
O diagrama foi estruturado para que a tabela fato F_Vendas estivesse conectada às tabelas dimensionais:
F_Vendas -> D_Calendário (pela coluna Date).
F_Vendas -> D_Produtos (pela coluna ID_Produto).
F_Vendas -> D_Produtos_Detalhes (pela coluna ID_Produto).
F_Vendas -> D_Descontos (pela coluna ID_Produto).
F_Vendas -> D_Detalhes (pela coluna ID_Produto).
* Nem todas as relações foram definidas como um para muitos (1:*), assegurando a conformidade com o modelo estrela queseria o ideal. Em algumas situações, foi necessário criar relacionamentos muitos para muitos (N:N). Embora o ideal em um modelo estrela seja usar (1:*) para manter a simplicidade e a performance do modelo, em certos casos, os dados exigiram essa abordagem para garantir que todas as informações fossem devidamente conectadas, especialmente onde as dimensões compartilham chaves em comum, como entre a tabela de descontos e detalhes de produtos.

