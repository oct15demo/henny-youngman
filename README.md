# henny-youngman
One Liners
----------

Example: From command line, list the columns of a csv file, ten versions provided below.
&nbsp;    
&nbsp;        

    awk -F, '{if(NR==1){for(i=1;i<=NF;i++){print $i;}}}'
      
    perl -e '{while(<>){foreach (split(",",$_)){if($prev){print $prev."\n";}$prev=$_;}print $prev;exit()}}'
    
    
    #simple, with generator, generator and formatting, ':>25' right aligns with 25 spaces
    
    python3 -c 'import sys;[print(r) for r in sys.stdin.readlines()[0][:-1].split(",")]'
    
    python3 -c 'import sys;[print(col) for col in next(r for r in sys.stdin.readlines())[:-1].split(",")]'
    
    '... sys;[print( f"{(col.strip()):>25}") for col in next(r for r in sys.stdin.readlines())[:-1].split(",")]'
    
    
    #csv simple,  with generator, with generator and formatting (python3 -c left out to show more of line)
    
    python3 -c 'import sys;import csv;[print(col) for col in [lines for lines in csv.reader(sys.stdin)][0]]'
    
    python3 -c 'import sys;import csv;[print(col) for col in next(line for line in csv.reader(sys.stdin))]'
    
    '... sys;import csv; [print(f"{col.strip():>25}") for col in next(line for line in csv.reader(sys.stdin))]'
    
    
    #csv with generator and formatting, plus example data of first row, formatting adds line feed if data > 100 in length for readability 
    'import sys;import csv; lines = (line for line in csv.reader(sys.stdin)); frow=lambda zrow:f"{(zrow[0].strip()):>25} | {zrow[1].strip()}"; [print(frow(col) + ("\n" if len(col[1])>100 else "")) for col in zip(next(lines),next(lines))]'

    #the same line above broken into components to make more understandable, semicolons, enclosing quotes omitted.
    import sys
    import csv
    lines = (line for line in csv.reader(sys.stdin))
    frow=lambda zrow:f"{(zrow[0].strip()):>25} | {zrow[1].strip()}"
    [print(frow(col) + ("\n" if len(col[1])>100 else "")) for col in zip(next(lines),next(lines))]
    
    
    #with pandas
    python -c 'import sys;import pandas;df=pandas.read_csv(sys.stdin);[print(col) for col in df.columns]'
    #with agate
    ... sys;import agate; table=agate.Table.from_csv(sys.stdin);[print(col) for col in table.column_names]'
     
&nbsp;  



There is also a project https://pypi.org/project/pawk/ (not mine obviously)
Powerful utility and yet small* codebase https://github.com/jasontrigg0/pawk/blob/master/pawk/pawk.py
*small excluding python itself and the libs used 

I started writing a utility I called awkthon, to enable awk syntax, but use case is dubious in light of pawk.

Python lines above don't include piped input, in order to view that code in github's limited viewing window without having to scroll. There are just 109 columns, or 29 better than 1928's 80 char limit, https://en.wikipedia.org/wiki/Punched_card#IBM_80-column_punched_card_format_and_character_codes
    
    1234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789
             10        20        30        40        50        60       70         80        90        100 
    
    
Entirety of command lines, shown below:
    #awk, pearl
    head daily.csv|awk -F, '{if(NR==1){for(i=1;i<=NF;i++){print $i;}}}'
    head daily.csv|perl -e '{while(<>){foreach (split(",",$_)){if($prev){print $prev."\n";}$prev=$_;}print $prev;exit()}}'
    
    #simple, simple formatted, simple csv, simple csv formatted
    head daily.csv|python -c 'import sys;[print(r) for r in sys.stdin.readlines()[0].split(",")[:-1]]'
    head daily.csv|python -c 'import sys;import csv;[print(col) for col in [lines for lines in csv.reader(sys.stdin)][0]]'
    head daily.csv|python -c 'import sys;import pandas;df=pandas.read_csv(sys.stdin);[print(col) for col in df.columns]'
    head daily.csv|python -c 'import sys;import agate; table=agate.Table.from_csv(sys.stdin);[print(col) for col in table.column_names]'
    
    #with generators, simple and formatted, and lastly, with first row of data
    cat daily.csv|python3 -c 'import sys;[print(col) for col in next(r for r in sys.stdin.readlines())[:-1].split(",")]'
    cat daily.csv|python3 -c 'import sys;[print( f"{(col.strip()):>25}") for col in next(r for r in sys.stdin.readlines())[:-1].split(",")]'
    cat daily.csv|python3 -c 'import sys;import csv;[print(col) for col in next(line for line in csv.reader(sys.stdin))]'
    cat daily.csv|python3 -c 'import sys;import csv; lines = (line for line in csv.reader(sys.stdin)); frow=lambda zrow:f"{(zrow[0].strip()):>25} | {zrow[1].strip()}"; [print(frow(col) + ("\n" if len(col[1])>100 else "")) for col in zip(next(lines),next(lines))]'
    

First ten lines of file daily.csv used in example.      

    date,state,positive,negative,pending,hospitalizedCurrently,hospitalizedCumulative,inIcuCurrently,inIcuCumulative,onVentilatorCurrently,onVentilatorCumulative,recovered,dataQualityGrade,lastUpdateEt,hash,dateChecked,death,hospitalized,total,totalTestResults,posNeg,fips,deathIncrease,hospitalizedIncrease,negativeIncrease,positiveIncrease,totalTestResultsIncrease
    20200516,AK,392,32889,,10,,,,,,344,B,5/16/2020 00:00,ebbc757dba7fd38b8459eb3908d4faa864d1d85a,2020-05-16T20:00:00Z,10,,33281,33281,33281,02,0,0,859,4,863
    20200516,AL,11523,141971,,,1387,,501,,295,,B,5/16/2020 00:00,b3b647c1555ac18188f23e454877c1b16b53caf2,2020-05-16T20:00:00Z,485,1387,153494,153494,153494,01,9,10,7124,307,7431
    20200516,AR,4578,77066,,65,520,,,10,101,3472,A,5/15/2020 16:20,2715a8779c1af1949901c1b256e0c7db130134ab,2020-05-16T20:00:00Z,98,520,81644,81644,81644,05,0,0,0,115,115
    20200516,AS,0,105,,,,,,,,,C,5/10/2020 00:00,b8a80308335510397208555fbb0dfad5bfc5416f,2020-05-16T20:00:00Z,0,,105,105,105,60,0,0,0,0,0
    20200516,AZ,13631,133157,,791,1683,344,,210,,3357,A+,5/16/2020 00:00,ed387914a563196c474d12d7bc518d1b58279ffd,2020-05-16T20:00:00Z,679,1683,146788,146788,146788,04,28,54,4325,462,4787
    20200516,CA,76793,1102333,,4424,,1313,,,,,B,5/16/2020 00:00,e335473bcecc2092fc63063fe12ed043c5f73751,2020-05-16T20:00:00Z,3204,,1179126,1179126,1179126,06,96,0,43363,1857,45220
    20200516,CO,21232,100608,,671,3842,,,,,3312,A,5/16/2020 00:00,460233581c589869dcbeee3780b77ac4d2f1a8de,2020-05-16T20:00:00Z,1150,3842,121840,121840,121840,08,59,53,3416,394,3810
    20200516,CT,36703,128052,,994,10946,,,,,6264,B,5/15/2020 20:15,b15eb6e4fb7fe44bff7ac2a1bfca9a81f05db0df,2020-05-16T20:00:00Z,3339,10946,164755,164755,164755,09,54,0,8229,618,8847
    20200516,DC,7042,28490,,382,,,,79,,998,A,5/15/202000:00,c2d074623d06cc96a1d5109a70bf3a1db3550010,2020-05-16T20:00:00Z,375,,35532,35532,35532,11,7,0,1022,171,1193
    
Output:

    date
    state
    positive
    negative
    pending
    hospitalizedCurrently
    hospitalizedCumulative
    inIcuCurrently
    inIcuCumulative
    onVentilatorCurrently
    onVentilatorCumulative
    recovered
    dataQualityGrade
    lastUpdateEt
    hash
    dateChecked
    death
    hospitalized
    total
    totalTestResults
    posNeg
    fips
    deathIncrease
    hospitalizedIncrease
    negativeIncrease
    positiveIncrease
    totalTestResultsIncrease
    
Source of file in example: https://covidtracking.com/api/v1/states/daily.csv

List the lengths of lines in a file:

    cat 0047.txt |python -c 'import sys; [print(len(line)) for line in sys.stdin.readlines()]'

     59
     60
     13
    513
    819
    434
    646
