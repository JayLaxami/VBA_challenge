Sub MultipleYearStockData():

    For Each ws In Worksheets
    
        Dim WorksheetName As String
        'Current row
        Dim i As Long
        'Start row of ticker block
        Dim j As Long
        'Index counter to fill Ticker row
        Dim TickerCount As Long
        'Last row column A
        Dim LastRowA As Long
        'last row column I
        Dim LastRowI As Long
        'Variable for percent change calculation
        Dim PercentChange As Double
        'Variable for greatest increase calculation
        Dim GreatestIncrease As Double
        'Variable for greatest decrease calculation
        Dim GreatestDecrease As Double
        'Variable for greatest total volume
        Dim GreatestVolume As Double
        
        'Get the WorksheetName
        WorksheetName = ws.Name
        
        'Create column headers
        ws.Cells(1, 9).Value = "Ticker"
        ws.Cells(1, 10).Value = "Yearly Change"
        ws.Cells(1, 11).Value = "Percent Change"
        ws.Cells(1, 12).Value = "Total Stock Volume"
        ws.Cells(1, 16).Value = "Ticker"
        ws.Cells(1, 17).Value = "Value"
        ws.Cells(2, 15).Value = "Greatest % Increase"
        ws.Cells(3, 15).Value = "Greatest % Decrease"
        ws.Cells(4, 15).Value = "Greatest Total Volume"
        
        'Set Ticker Counter to first row
        TickerCount = 2
        
        'Set start row to 2
        j = 2
        
        'Find the last non-blank cell in column A
        LastRowA = ws.Cells(Rows.Count, 1).End(xlUp).Row
        
        
            'Loop through all rows
            For i = 2 To LastRowA
            
                'Check if ticker name changed
                If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
                
                'Write ticker in column I
                ws.Cells(TickerCount, 9).Value = ws.Cells(i, 1).Value
                
                'Calculate and write Yearly Change in column J
                ws.Cells(TickerCount, 10).Value = ws.Cells(i, 6).Value - ws.Cells(j, 3).Value
                
                    'Conditional formating
                    If ws.Cells(TickerCount, 10).Value < 0 Then
                
                    'Set cell background color to red
                    ws.Cells(TickerCount, 10).Interior.ColorIndex = 3
                
                    Else
                
                    'Set cell background color to green
                    ws.Cells(TickerCount, 10).Interior.ColorIndex = 4
                
                    End If
                    
                    'Calculate and write percent change in column K
                    If ws.Cells(j, 3).Value <> 0 Then
                    PercentChange = ((ws.Cells(i, 6).Value - ws.Cells(j, 3).Value) / ws.Cells(j, 3).Value)
                    
                    'Percent formating
                    ws.Cells(TickerCount, 11).Value = Format(PercentChange, "Percent")
                    
                    Else
                    
                    ws.Cells(TickerCount, 11).Value = Format(0, "Percent")
                    
                    End If
                    
                'Calculate and write total volume in column L
                ws.Cells(TickerCount, 12).Value = WorksheetFunction.Sum(Range(ws.Cells(j, 7), ws.Cells(i, 7)))
                
                'Increase TickCount by 1
                TickerCount = TickerCount + 1
                
                'Set new start row of the ticker block
                j = i + 1
                
                End If
            
            Next i
            
        'Find last non-blank cell in column I
        LastRowI = ws.Cells(Rows.Count, 9).End(xlUp).Row
        
        
        'Prepare for summary
        GreatestVolume = ws.Cells(2, 12).Value
        GreatestIncrease = ws.Cells(2, 11).Value
        GreatestDecrease = ws.Cells(2, 11).Value
        
            'Loop for summary
            For i = 2 To LastRowI
            
                'For greatest total volume-check if next value is larger-if yes take over a new value and populate ws.Cells
                If ws.Cells(i, 12).Value > GreatestVolume Then
                GreatestVolume = ws.Cells(i, 12).Value
                ws.Cells(4, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatestVolume = GreatestVolume
                
                End If
                
                'For greatest increase-check if next value is larger-if yes take over a new value and populate ws.Cells
                If ws.Cells(i, 11).Value > GreatestIncrease Then
                GreatestIncrease = ws.Cells(i, 11).Value
                ws.Cells(2, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatestIncrease = GreatestIncrease
                
                End If
                
                'For greatest decrease-check if next value is smaller-if yes take over a new value and populate ws.Cells
                If ws.Cells(i, 11).Value < GreatestDecrease Then
                GreatestDecrease = ws.Cells(i, 11).Value
                ws.Cells(3, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatestDecrease = GreatestDecrease
                
                End If
                
            'Summary results in ws.Cells
            ws.Cells(2, 17).Value = Format(GreatestIncrease, "Percent")
            ws.Cells(3, 17).Value = Format(GreatestDecrease, "Percent")
            ws.Cells(4, 17).Value = Format(GreatestVolume, "Scientific")
            
            Next i
            
        'Adjust column width automatically
        Worksheets(WorksheetName).Columns("A:Z").AutoFit
            
    Next ws
        
End Sub

