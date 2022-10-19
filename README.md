## Challenge BI

Praticar o desenvolvimento de relatorios gerencias utilizando o Power BI

| :placard: Vitrine.Dev |     |
| -------------  | --- |
| :sparkles: Nome        | **Semana 1 - Logística**
| :label: Tecnologias | Power BI 
| :rocket: URL         | https://bit.ly/dashboarddelogística
| :fire: Desafio     | https://www.alura.com.br/challenges/bi

<!-- Inserir imagem com a #vitrinedev ao final do link -->
![image](https://user-images.githubusercontent.com/62486279/132950216-a3e4f253-ee01-4880-b9b4-634914f6f538.png#vitrinedev)

## Detalhes do projeto

O primeiro desafio proposto pela **Alura** através do **Challenge BI** (https://www.alura.com.br/challenges/bi), onde a empresa Alura Log (empresa fictícia) através do gerente logística solicitou a criação de um dashboard para acompanhamento de algumas métricas do setor, sendo elas:

- Quantas entregas foram feitas no prazo?

- Quantas entregas foram feitas atrasadas?

- Número de veículos disponíveis?

- Índice de ocorrências por estado?

- Nível médio de estoque por ano?

- Calcular o Ship to door (S2D), expedição até a entrega, medido em dias

E para simular um cenário mais real do dia a dia, foi utilizando o Trello: 

![image](https://user-images.githubusercontent.com/62486279/132996915-351af875-cbf3-4d03-9da2-eeca4a64e331.png)

Para controle das atividades através dos cartões e sistema Kanban 

## Como o report foi criado?

## 1) ETL e Relacionamento entre tabelas

Utilizando o Power Query, importamos quatro (4) bases de dados em formato .csv:

![image](https://user-images.githubusercontent.com/62486279/132951013-2c5820ce-0c3e-4e81-b155-110ee242dfd7.png)

Com cada base fornecendo as seguintes informações:

**Tabela - Estoque**

![image](https://user-images.githubusercontent.com/62486279/132950533-59b2ccac-c8fb-4e0c-a53d-11009bfa5406.png)

**Tabela - Pedidos**

![image](https://user-images.githubusercontent.com/62486279/132951074-02a6c6dd-d122-4898-8085-4073ceaf0dad.png)

![image](https://user-images.githubusercontent.com/62486279/132950600-23d620c3-d507-49e9-83d7-78e14bcc95d6.png)

**Tabela - Produtos**

![image](https://user-images.githubusercontent.com/62486279/132950613-e6253ffc-75d1-4b15-9f27-bb00534be9a3.png)

**Tabela - Veículos**

![image](https://user-images.githubusercontent.com/62486279/132950631-c8aff977-0368-4103-b6dc-d4508b227e32.png)

Alem de criar duas tabelas auxiliares:

**Tabela - Calendario**

![image](https://user-images.githubusercontent.com/62486279/132994896-462999f8-a9bf-4c0d-a472-c75d3c22d0a5.png)

\*Calendario criado de forma dinâmica através da coluna **Data da compra** da tabela de **Produtos** 

**Estados**

![image](https://user-images.githubusercontent.com/62486279/132950688-eae2c215-fa97-4b13-98f6-2b5ce09856f4.png)

Relacionando as tabelas da seguinte forma:

![image](https://user-images.githubusercontent.com/62486279/132950755-a63eb5b7-6fd7-4f0a-a334-85503c7e4dca.png)

E para cada base foi necessarios algumas tratativas, exemplo:

- Criado uma nova coluna concatenando o Nome do Mês e Ano e o Ano multiplicado por 100 mais o mes para auxiliar na organização da tabela calendario, para que a sequencia dos mês ficasse de forma correta (janeiro, fevereiro, março, etc);
- Tratar o coluna **Data atualização** da tabela **Estoque** onde a forma que estava não seria possivel realcionar com a tabela calendario; 
- Alterado o tipo do dado por localizado do coluna **Data da compra**
- Alterado os erros do coluna **Data da compra** para null
- Criado uma coluna de **data de entrega tratado** realizado uma validação com base no **status do pedido** 

  ![image](https://user-images.githubusercontent.com/62486279/132994394-af91dea7-4cbf-4627-ab1e-686e8af022e1.png)

- Criado uma coluna para calcular a diferença entre a coluna **Data previsão** menos a coluna **Data de entrega tratado**
- Criado uma coluna para calcular a diferença entre a coluna **Data de entrega tratado** menos a coluna **Data da compra**
- Separado o ID do produto da coluna **categoria_produto**
- Criado uma nova coluna com o nome do produto tratado sem underline

## 2) Calculos 

Para calcular as metricas necessarias, foi utilizados as medidas abaixo através de funções DAX:

Medida   | Dax | Comentário
-------- | ---------- | ----------
TotaldePedidos | COUNTA('Tabela - Pedidos'[Status do pedido]) | Medida para calcular o total de pedidos realizados 
EstoqueTotal | SUM('Tabela - Estoque'[Quantidade]) | Medida para calcular a quantidade em estoque 
MediaS2D |  Calculate(AVERAGE('Tabela - Pedidos'[S2D]),'Tabela - Pedidos'[Status Prazo] <> "Ainda não entregue") | Medida para calcular a média do Ship to door (S2D)
Entregadentrodoprazo | CALCULATE(COUNTROWS('Tabela - Pedidos'),'Tabela - Pedidos'[Status Prazo] = "Entrega dentro do prazo") | Medida para calcular os pedidos entregue dentro do prazo              
Entregaforadoprazo | CALCULATE(COUNTROWS('Tabela - Pedidos'),'Tabela - Pedidos'[Status Prazo] = "Entrega fora do prazo") | Medida para calcular os pedidos entregue dentro do prazo
TotalVeiculos | COUNTA('Tabela - Veículos'[Status]) | Medida para calcular o total de pedidos 
VeiculosOcupados | CALCULATE(COUNTROWS('Tabela - Veículos'),'Tabela - Veículos'[Status] = "Ocupado") | Medida para caclular a quantida de de veiculos ocupados 
VeiculosDisponivel | CALCULATE(COUNTROWS('Tabela - Veículos'),'Tabela - Veículos'[Status] = "Disponível") Medida para caclular a quantida de de veiculos disponiveis 

## Materiais de apoio 

**Livro:**

https://databinteligencia.com.br/produtos/dominando-o-power-bi/

**Sites:**

https://dev.to/alanfabricio/subindo-seu-repositorio-no-github-atraves-da-linha-de-comando-3kcm

https://www.alura.com.br/artigos/publicando-na-web-com-power-bi

**Videos:**

https://www.youtube.com/watch?v=DqTITcMq68k&list=PL9-wh4HdxPLbgoyk5HjlO7phM1eDeqFZD&index=1

