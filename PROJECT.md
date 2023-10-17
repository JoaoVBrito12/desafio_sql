import pandas as pd

# Leitura do CSV
df = pd.read_csv('DB_Teste.csv')
# Sumariza o valor vendido por cada vendedor, ordenando do maior para o menor
tabela_vendas_por_vendedor = df.groupby('Vendedor')['Valor'].sum().sort_values(ascending=False)

# Identifica o cliente responsável pela venda com o maior e menor valor
cliente_maior_valor = df[df['Valor'] == df['Valor'].max()]['Cliente'].values[0]
cliente_menor_valor = df[df['Valor'] == df['Valor'].min()]['Cliente'].values[0]

# Imprime a tabela de vendas por vendedor
print("Tabela de vendas por vendedor (ordenado do maior para o menor):")
print(tabela_vendas_por_vendedor)

print("\nCliente responsável pela venda com maior valor:", cliente_maior_valor)
print("Cliente responsável pela venda com menor valor:", cliente_menor_valor)
# Calcula o valor médio por tipo de venda
valor_medio_por_tipo_venda = df.groupby('Tipo')['Valor'].mean()

# Imprime o valor médio por tipo de venda
print("\nValor médio por Tipo de venda:")
print(valor_medio_por_tipo_venda)
# Conta o número de vendas por cliente
vendas_por_cliente = df['Cliente'].value_counts()

# Imprime o número de vendas realizadas por cada cliente
print("\nNúmero de vendas realizadas por cliente:")
print(vendas_por_cliente)


# Seção de Consultas SQL
CREATE TABLE Vendas (
    ID INT PRIMARY KEY,
    ClienteID INT,
    TipoID INT,
    DataVenda DATE,
    Categoria VARCHAR(50),
    VendedorID INT,
    DuraçãoContratoMeses INT,
    Equipe VARCHAR(50),
    Valor DECIMAL,
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ID),
    FOREIGN KEY (TipoID) REFERENCES TiposVenda(ID),
    FOREIGN KEY (VendedorID) REFERENCES Vendedores(ID)
);

CREATE TABLE Clientes (
    ID INT PRIMARY KEY,
    Nome VARCHAR(100)
);

CREATE TABLE Produtos (
    ID INT PRIMARY KEY,
    Nome VARCHAR(100)
);

CREATE TABLE TiposVenda (
    ID INT PRIMARY KEY,
    Tipo VARCHAR(50)
);

CREATE TABLE Vendedores (
    ID INT PRIMARY KEY,
    Nome VARCHAR(100)
);
SELECT V.ID AS VendaID, C.Nome AS ClienteNome
FROM Vendas V
JOIN Clientes C ON V.ClienteID = C.ID
WHERE YEAR(V.DataVenda) = 2020;
SELECT V.Nome AS VendedorNome, V.Equipe
FROM Vendedores V;
SELECT YEAR(DataVenda) AS Ano, 
       QUARTER(DataVenda) AS Trimestre, 
       SUM(Valor) AS TotalVendas
FROM Vendas
GROUP BY YEAR(DataVenda), QUARTER(DataVenda);
