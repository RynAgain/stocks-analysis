# stocks-analysis
 
## Overview of Project
### Purpose
The purpose of this coding project was to use VBA to analyze the financial data of multiple stocks over two years.  The goal was to make readable, efficient code that could be reused in the future.

### The Data
The data covers 12 different stocks over two years.  Each stock has a set of data divided into trading date, the prices, and trading volume.

## Results
### Analysis

The results between 2017 and 2018 show a stark difference between stock performance.  Specifically, 2017 yielded much better returns than 2018.  2018 had mostly negative returns with the exception of stock tickers "ENPH" and "RUN".

    Sub AllStocksAnalysisRefactored()
    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer

    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate

    Range("A1").Value = "All Stocks (" + yearValue + ")"

    'Create a header row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

    'Initialize array of all tickers
    Dim tickers(12) As String

    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"

    'Activate data worksheet
    Worksheets(yearValue).Activate

    'Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row

    '1a) Create a ticker Index
    tickerIndex = 0

    '1b) Create three output arrays
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12) As Single
    Dim tickerEndingPrices(12) As Single

    ''2a) Create a for loop to initialize the tickerVolumes to zero.
    ' If the next row’s ticker doesn’t match, increase the tickerIndex.
    For i = 0 To 11
    tickerVolumes(i) = 0
    tickerStartingPrices(i) = 0
    tickerEndingPrices(i) = 0
    Next i

    ''2b) Loop over all the rows in the spreadsheet.
    For i = 2 To RowCount

        '3a) Increase volume for current ticker
        tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value

        '3b) Check if the current row is the first row with the selected tickerIndex.
        'If  Then
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
            tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
        End If


        'End If

        '3c) check if the current row is the last row with the selected ticker
         'If the next row's ticker doesn't match, increase the tickerIndex.
        'If  Then
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
             tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
        End If


            '3d Increase the tickerIndex.
            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
                tickerIndex = tickerIndex + 1
            End If

        'End If

    Next i

    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    For i = 0 To 11

        Worksheets("All Stocks Analysis").Activate
        Cells(4 + i, 1).Value = tickers(i)
        Cells(4 + i, 2).Value = tickerVolumes(i)
        Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1


    Next i

    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.FontStyle = "Bold"
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.0%"
    Columns("B").AutoFit

    dataRowStart = 4
    dataRowEnd = 15

    For i = dataRowStart To dataRowEnd

        If Cells(i, 3) > 0 Then

            Cells(i, 3).Interior.Color = vbGreen

        Else

            Cells(i, 3).Interior.Color = vbRed

        End If

    Next i

    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

    End Sub



## Summary


### Advantages and Disadvantages of Refactoring Code
Advantages

•	Code is easy to read

•	Code Runs in less time

•	Refactored code is easier to debug.

•	Additional years can be analyzed with no issue

•	Additional tickers can easily be added if needed.

•	You can analyze multiple tickers at once.


### Disadvantages
•	Code becomes a one size fits all deal.  All future data must adhere to a specific data format

•	Tickers have to added to the code by hand.

### The Advantages and Disadvantages of Refactoring VBA Script
Decreased runtime and reusability are the biggest benefits fo this code. The refactored code runs much faster (see screenshots at the bottom) but this pales in comparison to the time saved by being able to reuse the code for future applications.


![2017_initial_runtime](https://github.com/RynAgain/stocks-analysis/blob/main/Resources/2017_initial_runtime.png)

2017_initial_runtime



![2017_final_runtime](https://github.com/RynAgain/stocks-analysis/blob/main/Resources/2017_Final_Runtime.png)

2017_final_runtime



![2018_initial_runtime](https://github.com/RynAgain/stocks-analysis/blob/main/Resources/2018_initial_runtime.png)

2018_initial_runtime



![2018_final_runtime](https://github.com/RynAgain/stocks-analysis/blob/main/Resources/2018_Final_Runtime.png)

2018_final_runtime