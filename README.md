# SQL_SERVER_DEFINICOES_EXEMPLOS

Este repositório contém uma compilação abrangente de definições e exemplos práticos para o ambiente SQL Server. Aqui, você encontrará informações detalhadas sobre conceitos fundamentais, funções avançadas e melhores práticas para otimizar o desempenho do seu sistema de gerenciamento de banco de dados.

## Conteúdo Destacado:

1. **Definições Essenciais:**
   - Explore as definições fundamentais do SQL Server, desde a criação de tabelas e índices até a gestão de chaves estrangeiras e procedimentos armazenados.

2. **Uso Eficiente de Views:**
   - Aprofunde-se no mundo das views e aprenda a criar visualizações poderosas para simplificar consultas complexas e fornecer camadas adicionais de segurança.

3. **Funções Avançadas:**
   - Descubra como criar funções personalizadas para realizar tarefas específicas, melhorando a modularidade e reutilização do código.

4. **Otimização de Consultas:**
   - Explore estratégias avançadas para otimizar consultas, incluindo o uso inteligente de índices, estatísticas e planos de execução.

5. **Soluções Criativas para Problemas Comuns:**
   - Enfrente desafios do mundo real com exemplos práticos, como a implementação de sistemas de inventário, identificação de tendências e recomendações de compra.

## Benefícios:
- Acesso fácil a definições precisas e exemplos claros.
- Suporte para usuários de todos os níveis, desde iniciantes até profissionais experientes.
- Demonstrações práticas para resolver problemas do mundo real com eficiência.

Este repositório visa capacitar os usuários a aprimorar suas habilidades em SQL Server, promovendo uma compreensão aprofundada das capacidades da plataforma e oferecendo soluções práticas para desafios do dia a dia no gerenciamento de banco de dados.

```SQL
-- 1. Definições Essenciais:
-- Criação de uma tabela simples
CREATE TABLE ExemploTabela (
    ID INT PRIMARY KEY,
    Nome VARCHAR(255),
    Idade INT,
    DataCadastro DATE
);

-- Adição de um índice à tabela
CREATE INDEX idx_Nome ON ExemploTabela(Nome);

-- Criação de uma tabela com chave estrangeira
CREATE TABLE Departamento (
    DeptID INT PRIMARY KEY,
    NomeDepartamento VARCHAR(50)
);

CREATE TABLE Funcionario (
    FuncID INT PRIMARY KEY,
    Nome VARCHAR(255),
    DeptID INT,
    FOREIGN KEY (DeptID) REFERENCES Departamento(DeptID)
);

-- Criação de um procedimento armazenado simples
CREATE PROCEDURE BuscarFuncionarioPorNome
    @Nome VARCHAR(255)
AS
BEGIN
    SELECT * FROM Funcionario WHERE Nome = @Nome;
END;

--2. Uso Eficiente de Views:
-- Criação de uma view para simplificar consultas
CREATE VIEW ViewClientesAtivos AS
SELECT ClienteID, Nome, Email
FROM Clientes
WHERE Status = 'Ativo';

-- Uso da view em uma consulta
SELECT * FROM ViewClientesAtivos;

-- Criação de uma view com camada adicional de segurança
CREATE VIEW ViewDadosFinanceiros AS
SELECT ClienteID, Nome, NULL AS NumeroCartaoCredito
FROM Clientes;

-- Uso da view com camada de segurança
SELECT * FROM ViewDadosFinanceiros;

--3. Funções Avançadas:
-- Criação de uma função para calcular o total de uma compra com desconto
CREATE FUNCTION CalcularTotalCompraComDesconto
    (@ValorTotal DECIMAL(10, 2), @Desconto DECIMAL(5, 2))
RETURNS DECIMAL(10, 2)
AS
BEGIN
    DECLARE @TotalComDesconto DECIMAL(10, 2);
    SET @TotalComDesconto = @ValorTotal - (@ValorTotal * @Desconto / 100);
    RETURN @TotalComDesconto;
END;

-- Uso da função em uma consulta
SELECT Produto, Preco, dbo.CalcularTotalCompraComDesconto(Preco, 10) AS PrecoComDesconto
FROM Produtos;

--4. Otimização de Consultas:
-- Criação de um índice para otimizar a consulta
CREATE INDEX idx_Produto ON Produtos(Produto);

-- Atualização de estatísticas
UPDATE STATISTICS Produtos;

-- Forçar um plano de execução específico
DECLARE @ProdutoBuscado VARCHAR(50) = 'Brinquedo';

SELECT *
FROM Produtos WITH (INDEX(idx_Produto))
WHERE Produto = @ProdutoBuscado;

--5. Soluções Criativas para Problemas Comuns:
-- Implementação de um sistema de inventário simples
CREATE TABLE Inventario (
    ProdutoID INT PRIMARY KEY,
    NomeProduto VARCHAR(255),
    QuantidadeEmEstoque INT
);

-- Identificação de tendências através de uma consulta
SELECT Categoria, COUNT(ProdutoID) AS Quantidade
FROM Inventario
GROUP BY Categoria
ORDER BY Quantidade DESC;

-- Recomendações de compra baseadas no estoque
SELECT NomeProduto, QuantidadeEmEstoque
FROM Inventario
WHERE QuantidadeEmEstoque > 0
ORDER BY NEWID(); -- Mistura a ordem para variedade nas recomendações

-- Conceitos gerais

--FUNÇÕES DE TEXTO

--Função UPPER: Converte uma string em letras maiúsculas.
SELECT UPPER('exemplo') AS texto_maiusculo;

--Função LOWER: Converte uma string em letras minúsculas.
SELECT LOWER('EXEMPLO') AS texto_minusculo;

--Função REPLACE: Substitui todas as ocorrências de uma substring por outra substring em uma string.
SELECT REPLACE('banana', 'na', 'ra') AS texto_substituido;

--Função LEN: Retorna o comprimento (número de caracteres) de uma string.
SELECT LEN('Hello, world!') AS comprimento_string;

--Função LEFT: Retorna os primeiros caracteres de uma string com base em um comprimento especificado.
SELECT LEFT('Hello, world!', 5) AS caracteres_esquerdos;

--Função RIGHT: Retorna os últimos caracteres de uma string com base em um comprimento especificado.
SELECT RIGHT('Hello, world!', 6) AS caracteres_direitos;

--Função SUBSTRING: Extrai uma parte de uma string com base em uma posição inicial e comprimento.
SELECT SUBSTRING('abcdefg', 2, 4) AS substring_texto;

--Função CHARINDEX: Retorna a posição da primeira ocorrência de uma substring dentro de uma string.
SELECT CHARINDEX('na', 'banana') AS posicao_substring;

--Função LTRIM: Remove espaços em branco no início de uma string. 
SELECT LTRIM('   Hello') AS texto_ltrim;

--Função RTRIM: Remove espaços em branco no final de uma string.
SELECT RTRIM('Hello   ') AS texto_rtrim;

--Função TRIM: Remove espaços em branco no início e no final de uma string.
SELECT TRIM('   Olá   ') AS texto_trim;

--Função CONCAT: Concatena duas ou mais strings em uma única string.
SELECT CONCAT('Olá', ', ', 'mundo!') AS texto_concatenado;

--Função REPLICATE: Repete uma string um determinado número de vezes.
SELECT REPLICATE('abc', 3) AS texto_repetido;

--Função STUFF: Substitui uma parte de uma string por outra string em uma posição específica.
SELECT STUFF('Hello, world!', 8, 5, 'Universe') AS texto_substituido;

--Função PATINDEX: Retorna a posição da primeira ocorrência de um padrão de 
--expressão regular em uma string.
SELECT PATINDEX('%world%', 'Hello, world!') AS posicao_padrao;

--Função SPACE: Retorna uma string de espaços em branco com um comprimento especificado.
SELECT SPACE(5) AS espacos_em_branco;

--Função QUOTENAME: Adiciona delimitadores [ ] a uma string para torná-la um nome de objeto válido.
SELECT QUOTENAME('MyTable') AS nome_delimitado;

--Função TRANSLATE: Substitui caracteres em uma string com base em um mapeamento definido.
SELECT TRANSLATE('Olá, mundo!', 'á', 'a') AS texto_substituido;

--Função ASCII: Retorna o valor ASCII do primeiro caractere de uma string.
SELECT ASCII('A') AS valor_ascii;

--Função FORMAT: Formata um valor numérico com base em um padrão especificado. 
SELECT FORMAT(1234.56789, '0,0.00') AS valor_formatado;

--Função UNICODE: Retorna o valor Unicode do primeiro caractere de uma string. 
SELECT UNICODE('A') AS valor_unicode;

--Função SOUNDEX: Retorna um código fonético de quatro caracteres para uma string.
SELECT SOUNDEX('Hello') AS codigo_fonetico;

--Função DIFFERENCE: Retorna uma pontuação de diferença entre duas strings 
--com base no algoritmo de comparação fonética.
SELECT DIFFERENCE('Hello', 'Hallo') AS pontuacao_diferenca;

--*************************************************************************************--

--FUNÇÃO DE DATA/HORA

--Função GETDATE: Retorna a data e a hora atuais.
SELECT GETDATE() AS data_hora_atual;
SELECT CURRENT_TIMESTAMP AS data_hora_atual;

--Função DATEADD: Adiciona uma quantidade especificada de tempo a uma data ou hora. 
SELECT DATEADD(MONTH, 1, '2023-06-13') AS data_mais_um_mes;

--Função DATEDIFF: Calcula a diferença entre duas datas ou horas em uma unidade específica 
--(anos, meses, dias, etc.).
SELECT DATEDIFF(DAY, '2023-06-10', '2023-06-15') AS diferenca_dias;

--Função DATEPART: Extrai uma parte específica (ano, mês, dia, etc.) de uma data ou hora. 
SELECT DATEPART(YEAR, '2023-06-13') AS ano;

--Função FORMAT: Formata uma data ou hora de acordo com um padrão especificado. 
SELECT FORMAT(GETDATE(), 'dd/MM/yyyy HH:mm:ss') AS data_hora_formatada;

--Função DAY, MONTH ou YEAR: Retorna o dia, mês ou o ano de uma data.
SELECT DAY('2023-06-13') AS dia;
SELECT MONTH('2023-06-13') AS mes;
SELECT YEAR('2023-06-13') AS ano;

--DATENAME retorna o nome por extenso do DATEPART da data.
SELECT NOME + ' nasceu em ' + 
DATENAME (WEEKDAY,DATA_DE_NASCIMENTO) + ',' +
DATENAME (DAY,DATA_DE_NASCIMENTO) + ' de ' +
DATENAME(MONTH, DATA_DE_NASCIMENTO) + ' de ' +
DATENAME(YEAR, DATA_DE_NASCIMENTO) AS DATA_EXTENSO
FROM TABELA_DE_CLIENTES;

--*************************************************************************************--

--FUNÇÃO DE NUMÉRICA

--Função SUM: Calcula a soma de valores em uma coluna.
SELECT SUM(valor) AS soma_valores FROM tabela;

--Função AVG: Calcula a média dos valores em uma coluna.
SELECT AVG(valor) AS media_valores FROM tabela;

--Função MAX: Retorna o valor máximo em uma coluna.
SELECT MAX(valor) AS valor_maximo FROM tabela;

--Função MIN: Retorna o valor mínimo em uma coluna.
SELECT MIN(valor) AS valor_minimo FROM tabela;

--Função COUNT: Retorna o número de linhas em uma coluna.
SELECT COUNT(*) AS total_linhas FROM tabela;

--Função RAND: Retorna um número aleatório dentro de um intervalo especificado. 
SELECT RAND() * 100 AS numero_aleatorio;

--Função ABS: Retorna o valor absoluto de um número.
SELECT ABS(-10) AS valor_absoluto;

--Função ROUND: Arredonda um número para o inteiro mais próximo.
SELECT ROUND(3.721,2) AS numero_arredondado;

--Função CEILING: Arredonda um número para o próximo número inteiro maior ou igual. 
SELECT CEILING(4.3) AS numero_arredondado;

--Função FLOOR: Arredonda um número para o próximo número inteiro menor ou igual. 
SELECT FLOOR(4.7) AS numero_arredondado;

--Função SQRT: Calcula a raiz quadrada de um número.
SELECT SQRT(25) AS raiz_quadrada;

--Função POWER: Calcula um número elevado a uma potência específica. 
SELECT POWER(2, 3) AS resultado_potencia;

--Função SIGN: Retorna o sinal de um número 
--(-1 para números negativos, 0 para zero, 1 para números positivos).
SELECT SIGN(-10) AS sinal;

--Função EXP: Calcula o valor exponencial de um número.
SELECT EXP(2) AS valor_exponencial;

--Função LOG: Calcula o logaritmo natural (base e) de um número.
SELECT LOG(10) AS logaritmo;

--Função SIN: Retorna o seno de um ângulo em radianos.
SELECT SIN(0) AS seno;

--Função COS: Retorna o cosseno de um ângulo em radianos. 
SELECT COS(0) AS cosseno;

--Função TAN: Retorna a tangente de um ângulo em radianos.
SELECT TAN(0) AS tangente;

--Função PI: Retorna o valor de π (pi).
SELECT PI() AS valor_pi;

--Função DEGREES: Converte um ângulo de radianos para graus.
SELECT DEGREES(1.5708) AS graus;

-- Definições rápidas:

SELECT: Recupera dados das tabelas
ORDER BY: Ordena os dados em ascendente ou descendente
UNION: Combina os resultados das consultas em uma única tabela
WHERE: Filtra os dados a partir de uma condição
HAvING: Filtra os resultados de uma cláusula GROUP BY
JOIN: Combina linhas das tabelas com base em uma condição
LIKE: Realiza uma busca de padrão em uma coluna (Jo%) (%Jo) 
 TRUNCATE: Remove todos os registros de uma tabela, mas mantém a estrutura da tabela para reutilização.
 CONSTRAINT: Define regras ou restrições para os dados em uma tabela, como chaves primárias, chaves estrangeiras e restrições de verificação.

Escape Character:
Às vezes, pode ser necessário buscar por valores que contêm os próprios coringas como parte da string. 
Nesses casos, utiliza-se o comando ESCAPE para tratar os coringas literalmente.
SELECT * FROM Tabela WHERE Descricao LIKE '10!%' ESCAPE '!' -- Buscaria registros começando com '10%'

IN: Realiza uma busca de padrão em uma coluna
BETWEEN: Filtra resultados dentro de um intervalo específico
IS NULL: Retorna valores nulos
IS NOT NULL: Retorna valores que não são nulos
DISTINCT: Remove duplicatas dos resultados da consulta
EXISTS: Verifica se existe pelo menos um registro na subconsulta
NOT EXISTS: Verifica se NÃO existe pelo menos um registro na subconsulta
DATE_FORMAT: Formata data no formato desejado
NOW(): Retorna a hora atual do servidor
LIMIT: Restringe o número de linhas retornadas

INNER JOIN: Combinação de registros com correspondência em ambas as tabelas
LEFT JOIN: Combinação de registros com correspondência nas duas tabelas e inclui registros da esquerda
RIGHT JOIN: Combinação de registros com correspondência nas duas tabelas e inclui registros da direita
FULL OUTER JOIN: Combinação de registros com correspondência nas duas tabelas e inclui registros da esquerda e direita

SUM(): Soma os valores de uma coluna.
AVG(): Calcula a média dos valores de uma coluna.
COUNT(): Conta o número de linhas em um conjunto de resultados.
MIN() e MAX(): Encontram o valor mínimo e máximo em uma coluna, respectivamente.
GROUP BY: Agrupa os resultados com base em uma ou mais colunas.
ROW_NUMBER(), RANK() e DENSE_RANK(): Atribuem números a linhas em um conjunto de resultados ordenado.
LEAD() e LAG(): Acessam dados de linhas subsequentes ou anteriores em um conjunto de resultados ordenado.
OVER(): Define a janela sobre a qual as funções de janela operam.
LEN(): Retorna o comprimento de uma string.
LEFT() e RIGHT(): Retorna parte de uma string da esquerda ou direita.
CHARINDEX(): Encontra a posição de uma substring em uma string.
CONCAT(): Concatena strings.
GETDATE(): Retorna a data e hora atuais.
DATEDIFF(): Calcula a diferença entre duas datas.
DATEPART(): Retorna uma parte específica de uma data (dia, mês, ano, etc.).
FORMAT(): Formata uma data de acordo com um formato especificado.
CASE WHEN: Realiza avaliações condicionais em uma coluna ou expressão.
COALESCE(): Retorna o primeiro valor não nulo em uma lista.
CAST() e CONVERT(): Convertem um tipo de dado em outro.

```
