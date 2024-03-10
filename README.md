let
    #"DataMin" = List.Min(BaseVendas[Data da Venda]),
    DataMax = List.Max(BaseVendas[Data da Venda]),
    QtdDias = Duration.Days(DataMax-DataMin)+1,
    Personalizar1 = List.Dates(DataMin,QtdDias, #duration(1, 0, 0, 0)),
    #"Convertido para Tabela" = Table.FromList(Personalizar1, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Colunas Renomeadas" = Table.RenameColumns(#"Convertido para Tabela",{{"Column1", "Datas"}}),
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Colunas Renomeadas",{{"Datas", type date}}),
    #"Ano Inserido" = Table.AddColumn(#"Tipo Alterado", "Ano", each Date.Year([Datas]), Int64.Type),
    #"Mês Inserido" = Table.AddColumn(#"Ano Inserido", "Mês", each Date.Month([Datas]), Int64.Type),
    #"Dia Inserido" = Table.AddColumn(#"Mês Inserido", "Dia", each Date.Day([Datas]), Int64.Type),
    #"Nome do Mês Inserido" = Table.AddColumn(#"Dia Inserido", "Nome do Mês", each Date.MonthName([Datas]), type text),
    #"Colunas Reordenadas" = Table.ReorderColumns(#"Nome do Mês Inserido",{"Datas", "Ano", "Mês", "Nome do Mês", "Dia"}),
    #"Colunas Removidas" = Table.RemoveColumns(#"Colunas Reordenadas",{"Dia"}),
    #"Início do Mês Inserido" = Table.AddColumn(#"Colunas Removidas", "Início do Mês", each Date.StartOfMonth([Datas]), type date),
    #"Trimestre Inserido" = Table.AddColumn(#"Início do Mês Inserido", "Trimestre", each Date.QuarterOfYear([Datas]), Int64.Type),
    #"Início do Trimestre Inserido" = Table.AddColumn(#"Trimestre Inserido", "Início do Trimestre", each Date.StartOfQuarter([Datas]), type date)
in
    #"Início do Trimestre Inserido"
