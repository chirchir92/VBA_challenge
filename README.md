' The VBA of Wall Street code

Sub WallstreetAnalysis():

    ' Looping Through All Worksheets
    For Each w_sheet In Worksheets

        'naming the colum and row fields
        w_sheet.Range("I1").Value = "Ticker"
        w_sheet.Range("J1").Value = "Yearly Change"
        w_sheet.Range("K1").Value = "Percent Change"
        w_sheet.Range("L1").Value = " Total Stock Volume"
        

                'setting initial variables for holding variables in a worksheet
                Dim TickerName As String
                Dim lastrow As Long 'since we cannot determine the last row in each worksheet due to large dataset
                Dim TotalTickerVolume As Double
                TotalTickerVolume = 0
                Dim SummaryTableRow As Long
                SummaryTableRow = 2
                Dim YearlyOpen As Double
                Dim YearlyClose As Double
                Dim YearlyChange As Double
                Dim previousamount As Long
                previousamount = 2
                Dim PercentChange As Double
                
                
                'naming column and row fields in the summary table variables
                w_sheet.Range("O2").Value = "Greatest % Increase"
                w_sheet.Range("O3").Value = "Greatest % Decrease"
                w_sheet.Range("O4").Value = "Greatest Total Volume"
                w_sheet.Range("P1").Value = "Ticker"
                w_sheet.Range("Q1").Value = "Value"
                
                    'setting initial variables for holding variables in the second summary table
                    Dim GreatestDecrease As Double
                    GreatestDecrease = 0
                    Dim GreatestIncrease As Double
                    GreatestIncrease = 0
                    Dim LastRowValue As Long
                    Dim GreatestTotalVolume As Double
                    GreatestTotalVolume = 0

                'determining the last row
                lastrow = w_sheet.Cells(Rows.Count, 1).End(xlUp).Row 'selects the last non-empty row and all the values upwards
        
                        For i = 2 To lastrow

                             'summing up the total volume
                            TotalTickerVolume = TotalTickerVolume + w_sheet.Cells(i, 7).Value
                             
                             'check if we are still dealing with the same ticker
                            If w_sheet.Cells(i + 1, 1).Value <> w_sheet.Cells(i, 1).Value Then


                            'setting ticker name
                            TickerName = w_sheet.Cells(i, 1).Value
                            
                            'print ticker name
                            w_sheet.Range("I" & SummaryTableRow).Value = TickerName
                            
                            
                            'print the ticker total amount at the chosen column
                            w_sheet.Range("L" & SummaryTableRow).Value = TotalTickerVolume
                            TotalTickerVolume = 0 'reset ticker total

                            'set opening and closing price and yearly change name
                            YearlyOpen = w_sheet.Range("C" & previousamount)
                            YearlyClose = w_sheet.Range("F" & i)
                            YearlyChange = YearlyClose - YearlyOpen
                            w_sheet.Range("J" & SummaryTableRow).Value = YearlyChange

                                'calculating the percentage change
                                 If YearlyOpen = 0 Then
                                    PercentChange = 0
                                 Else
                                    YearlyOpen = w_sheet.Range("C" & previousamount)
                                    PercentChange = YearlyChange / YearlyOpen
                                
                                End If
                
                
                                    'converts the double data-type at column K to % and sets two decimal points
                                    w_sheet.Range("K" & SummaryTableRow).NumberFormat = "0.00%"
                                    w_sheet.Range("K" & SummaryTableRow).Value = PercentChange

                                         '  conditional formatting of the positives with green and negatives with red
                                         If w_sheet.Range("J" & SummaryTableRow).Value >= 0 Then
                                            w_sheet.Range("J" & SummaryTableRow).Interior.ColorIndex = 4 'color code for green
                                        Else
                                            w_sheet.Range("J" & SummaryTableRow).Interior.ColorIndex = 3 'color code for green
                                        End If
            
                                        'add one to the table
                                        SummaryTableRow = SummaryTableRow + 1
                                    previousamount = i + 1
                                End If
            Next i


            'sets the final row for the % increase and decrease at column 11
            lastrow = w_sheet.Cells(Rows.Count, 11).End(xlUp).Row
        
            ' looping through final results
            For i = 2 To lastrow
                If w_sheet.Range("K" & i).Value > w_sheet.Range("Q2").Value Then
                    w_sheet.Range("Q2").Value = w_sheet.Range("K" & i).Value
                    w_sheet.Range("P2").Value = w_sheet.Range("I" & i).Value
                End If

                If w_sheet.Range("K" & i).Value < w_sheet.Range("Q3").Value Then
                    w_sheet.Range("Q3").Value = w_sheet.Range("K" & i).Value
                    w_sheet.Range("P3").Value = w_sheet.Range("I" & i).Value
                End If

                If w_sheet.Range("L" & i).Value > w_sheet.Range("Q4").Value Then
                    w_sheet.Range("Q4").Value = w_sheet.Range("L" & i).Value
                    w_sheet.Range("P4").Value = w_sheet.Range("I" & i).Value
                End If

            Next i
        ' since Q2&3 had double as data typehis command puts % and a two decimal point
            w_sheet.Range("Q2").NumberFormat = "0.00%"
            w_sheet.Range("Q3").NumberFormat = "0.00%"
            
        
        w_sheet.Columns("I:Q").AutoFit 'autofitting the table column at column I and Q

    Next w_sheet

End Sub

