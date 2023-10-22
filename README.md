# DESAFIO-POWERBI-DIO

Código em linguagem M que você pode usar para manipular os dados de vendas no Power Query do Excel:

````
let
    // Passo 1: Importar os Dados de Vendas
    Source = Excel.Workbook(File.Contents("C:\Caminho\Para\Seu\Arquivo.xlsx"), null, true),
    Sheet1_Sheet = Source{[Item="Sheet1",Kind="Sheet"]}[Data],
    #"Linhas Iniciais Ignoradas" = Table.Skip(Sheet1_Sheet,1),
    
    // Passo 2: Limpar e Transformar os Dados
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Linhas Iniciais Ignoradas",{{"Data", type date}, {"Vendas", type number}, {"Produto", type text}}),
    #"Linhas em Branco Removidas" = Table.SelectRows(#"Tipo Alterado", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    
    // Passo 3: Agregar os Dados
    #"Linhas Agrupadas" = Table.Group(#"Linhas em Branco Removidas", {"Produto"}, {{"Vendas Totais", each List.Sum([Vendas]), type number}}),
    
    // Passo 4: Carregar os Dados na Planilha
    #"Consultas Combinadas" = Table.Combine({#"Linhas em Branco Removidas", #"Linhas Agrupadas"}),
    
    // Passo 5: Criar o Relatório de Vendas
    #"Relatório Criado" = #"Consultas Combinadas",
    
    // Passo 6: Atualizar o Relatório Regularmente
    Fonte = Excel.CurrentWorkbook(){[Name="Relatório_Criado"]}[Content]
in
    Fonte

    ````

Substitua "C:\Caminho\Para\Seu\Arquivo.xlsx" pelo caminho real do seu arquivo de dados de vendas. Além disso, ajuste os passos de acordo com as especificações e necessidades específicas do seu relatório de vendas.
