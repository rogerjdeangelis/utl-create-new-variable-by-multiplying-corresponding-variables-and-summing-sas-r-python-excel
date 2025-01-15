# utl-create-new-variable-by-multiplying-corresponding-variables-and-summing-sas-r-python-excel
Create new variable by multiplying corresponding variables and summing sas r python excel
    %let pgm=utl-create-new-variable-by-multiplying-corresponding-variables-and-summing-sas-r-python-excel;

    Create new variable by multiplying corresponding variables and summing sas r python excel

            SOLUTIONS

                1  generate sql
                2  sas sql
                3  r sql
                4  python sql
                5  excel sql
                6  r rowsum(a*b)

    SOAPBOX ON

      Mixed case language syntax is very hard to commit to memory

      Consider these variations

       1. rowsum(matrix), rowSums(matrix and dataframe)

       2. nrow and NROW are genuine R functions
          n()-tidyverse(with mutate summarize), nrows, nRow, and NRows
          I suspect same issue with python.

       3  Sometimes having explicit sql code is more transparent
          and production level?  Avoid select * in production?

    SOAPBOX OFF

    github
    https://tinyurl.com/u3bf37xh
    https://github.com/rogerjdeangelis/utl-create-new-variable-by-multiplying-corresponding-variables-and-summing-sas-r-python-excel

    stackoverflow
    https://tinyurl.com/5x25tb2f
    https://stackoverflow.com/questions/79334110/create-new-variable-by-multiplying-corresponding-variables-and-summing

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                   |                                          |                                         */
    /*             INPUT                 |       PROCESS                            |          OUTPUT                         */
    /*             =====                 |       =======                            |          ======                         */
    /*                                   |                                          |                                         */
    /*   SD1.HAVE                        | Add Column smab                          |                                         */
    /*                                   | sum(A1*B1+A2*B2+A3*B3+A4*B4)             |  A1 A2 A3 A4 B1 B2 B3 B4 SUMAB          */
    /*   A1 A2 A3 A4 B1 B2 B3 B4         |                                          |                                         */
    /*                                   | 1 USE SAS TO GENERATE SQL CODE           |  13 15 15 11 13 15 15 11   740          */
    /*   13 15 15 11 13 15 15 11         |   could imbed code in sql                |  13 14 13 11 13 14 13 11   655          */
    /*   13 14 13 11 13 14 13 11         | ===============================          |  12 11 13 15 12 11 13 15   659          */
    /*   12 11 13 15 12 11 13 15         |                                          |  12 12 11 13 12 12 11 13   578          */
    /*   12 12 11 13 12 12 11 13         |   %let _vrs=4; /*-- vars --*/            |  13 13 14 12 13 13 14 12   678          */
    /*   13 13 14 12 13 13 14 12         |   %array(_ix,values=1-&_vrs);            |                                         */
    /*                                   |                                          |                                         */
    /*                                   |   data _null_;                           |                                         */
    /*   options validvarname=upcase;    |     %do_over(_ix,phrase=                 |                                         */
    /*   ibname sd1 "d:/sd1";            |        %str(put "a? * b? + ";));         |                                         */
    /*   ata sd1.have;                   |   run;quit;                              |                                         */
    /*   nput                            |                                          |                                         */
    /*    a1 a2 a3 a4 b1 b2 b3 b4;       |   cur an paste from log                  |                                         */
    /*   ards4;                          |                                          |                                         */
    /*   3 15 15 11 13 15 15 11          |   a1 * b1 as ab1 +                       |                                         */
    /*   3 14 13 11 13 14 13 11          |   a2 * b2 as ab2 +                       |                                         */
    /*   2 11 13 15 12 11 13 15          |   a3 * b3 as ab3 +                       |                                         */
    /*   2 12 11 13 12 12 11 13          |   a4 * b4 as ab4 +                       |                                         */
    /*   3 13 14 12 13 13 14 12          |                                          |                                         */
    /*   ;;;                             |                                          |                                         */
    /*   un;quit;                        |                                          |                                         */
    /*                                   |  2-5 SAME IN SAS R PYTHON EXCEL          |                                         */
    /*                                   |  ==============================          |                                         */
    /*                                   |                                          |                                         */
    /*                                   |    select                                |                                         */
    /*                                   |       *                                  |                                         */
    /*                                   |      ,a1 * b1 +                          |                                         */
    /*                                   |       a2 * b2 +                          |                                         */
    /*                                   |       a3 * b3 +                          |                                         */
    /*                                   |       a4 * b4  as sumab                  |                                         */
    /*                                   |    from                                  |                                         */
    /*                                   |       sd1.have                           |                                         */
    /*                                   |                                          |                                         */
    /*                                   |                                          |                                         */
    /*                                   |  6  R ROWSUM(A*B)                        |                                         */
    /*                                   |  ================                        |                                         */
    /*                                   |                                          |                                         */
    /*                                   |  Took me a little time because           |                                         */
    /*                                   |                                          |                                         */
    /*                                   |    1 had to look up rowSums function     |                                         */
    /*                                   |    2 had to convert from                 |                                         */
    /*                                   |      dataframe to matrix and back        |                                         */
    /*                                   |      to dataframe                        |                                         */
    /*                                   |    3 cbind(matrix,dataframe) yeids       |                                         */
    /*                                   |      does not work?                      |                                         */
    /*                                   |                                          |                                         */
    /*                                   |    have<-as.matrix(                      |                                         */
    /*                                   |       read_sas("d:/sd1/have.sas7bdat"))  |                                         */
    /*                                   |    want <- data.frame(sumab=             |                                         */
    /*                                   |       rowSums(have[,1:4]*have[,5:8]))    |                                         */
    /*                                   |    want<-cbind(want,have)                |                                         */
    /*                                   |                                          |                                         */
    /*                                   |    LESS IS MORE                          |                                         */
    /*                                   |                                          |                                         */
    /**************************************************************************************************************************/

    /*                                   _                   _
    / |   __ _  ___ _ __   ___ _ __ __ _| |_ ___   ___  __ _| |
    | |  / _` |/ _ \ `_ \ / _ \ `__/ _` | __/ _ \ / __|/ _` | |
    | | | (_| |  __/ | | |  __/ | | (_| | ||  __/ \__ \ (_| | |
    |_|  \__, |\___|_| |_|\___|_|  \__,_|\__\___| |___/\__, |_|
         |___/                                            |_|
    */


    %let _vrs=4; /*-- vars --*/
    %array(_ix,values=1-&_vrs);

    data _null_;
      %do_over(_ix,phrase=
         %str(put "a? * b? + ";));
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  CUT AND PASTE FROM SAS LOG                                                                                            */
    /*                                                                                                                        */
    /*  a1 * b1 +                                                                                                             */
    /*  a2 * b2 +                                                                                                             */
    /*  a3 * b3 +                                                                                                             */
    /*  a4 * b4 +                                                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                              _
    |___ \   ___  __ _ ___   ___  __ _| |
      __) | / __|/ _` / __| / __|/ _` | |
     / __/  \__ \ (_| \__ \ \__ \ (_| | |
    |_____| |___/\__,_|___/ |___/\__, |_|
                                    |_|
    */

    proc sql;
      create
         table want as
      select
         *
        ,a1 * b1 +
         a2 * b2 +
         a3 * b3 +
         a4 * b4  as sumab
      from
         sd1.have
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   A1    A2    A3    A4    B1    B2    B3    B4    SUMAB                                                                */
    /*                                                                                                                        */
    /*   13    15    15    11    13    15    15    11     740                                                                 */
    /*   13    14    13    11    13    14    13    11     655                                                                 */
    /*   12    11    13    15    12    11    13    15     659                                                                 */
    /*   12    12    11    13    12    12    11    13     578                                                                 */
    /*   13    13    14    12    13    13    14    12     678                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                    _
    |___ /   _ __   ___  __ _| |
      |_ \  | `__| / __|/ _` | |
     ___) | | |    \__ \ (_| | |
    |____/  |_|    |___/\__, |_|
                           |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    want<-sqldf('
      select
         *
        ,a1 * b1 +
         a2 * b2 +
         a3 * b3 +
         a4 * b4  as sumab
      from
         have
      ')
    want;
    print(have)
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                 |                                                                                      */
    /* R                               |  SAS                                                                                 */
    /*                                 |                                                                                      */
    /* A1 A2 A3 A4 B1 B2 B3 B4 sumab   |  ROWNAMES    A1    A2    A3    A4    B1    B2    B3    B4    SUMAB                   */
    /*                                 |                                                                                      */
    /* 13 15 15 11 13 15 15 11   740   |      1       13    15    15    11    13    15    15    11     740                    */
    /* 13 14 13 11 13 14 13 11   655   |      2       13    14    13    11    13    14    13    11     655                    */
    /* 12 11 13 15 12 11 13 15   659   |      3       12    11    13    15    12    11    13    15     659                    */
    /* 12 12 11 13 12 12 11 13   578   |      4       12    12    11    13    12    12    11    13     578                    */
    /* 13 13 14 12 13 13 14 12   678   |      5       13    13    14    12    13    13    14    12     678                    */
    /*                                 |                                                                                      */
    /**************************************************************************************************************************/

    /*  _                 _   _                             _
    | || |    _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    | || |_  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
    |__   _| | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
       |_|   | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
             |_|    |___/                                |_|
    */

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    want=pdsql('''
       select               \
          *                 \
         ,a1 * b1 +         \
          a2 * b2 +         \
          a3 * b3 +         \
          a4 * b4  as sumab \
       from                 \
          have              \
       ''');
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* PYTHON                                                                                                                 */
    /*                                                                                                                        */
    /*      A1    A2    A3    A4    B1    B2    B3    B4  sumab                                                               */
    /*                                                                                                                        */
    /* 0  13.0  15.0  15.0  11.0  13.0  15.0  15.0  11.0  740.0                                                               */
    /* 1  13.0  14.0  13.0  11.0  13.0  14.0  13.0  11.0  655.0                                                               */
    /* 2  12.0  11.0  13.0  15.0  12.0  11.0  13.0  15.0  659.0                                                               */
    /* 3  12.0  12.0  11.0  13.0  12.0  12.0  11.0  13.0  578.0                                                               */
    /* 4  13.0  13.0  14.0  12.0  13.0  13.0  14.0  12.0  678.0                                                               */
    /*                                                                                                                        */
    /* same as before in sas                                                                                                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                      _             _
    | ___|    _____  _____ ___| |  ___  __ _| |
    |___ \   / _ \ \/ / __/ _ \ | / __|/ _` | |
     ___) | |  __/>  < (_|  __/ | \__ \ (_| | |
    |____/   \___/_/\_\___\___|_| |___/\__, |_|
     _                   _         _      |_|         _
    (_)_ __  _ __  _   _| |_   ___| |__   ___  ___| |_
    | | `_ \| `_ \| | | | __| / __| `_ \ / _ \/ _ \ __|
    | | | | | |_) | |_| | |_  \__ \ | | |  __/  __/ |_
    |_|_| |_| .__/ \__,_|\__| |___/_| |_|\___|\___|\__|
            |_|
    */


    %utlfkil(d:/xls/wantxl.xlsx);

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    library(haven)
    have<-read_sas("d:/sd1/have.sas7bdat")
    have
    wb <- createWorkbook()
    addWorksheet(wb, "have")
    writeData(wb, sheet = "have", x = have)
    saveWorkbook(
        wb
       ,"d:/xls/wantxl.xlsx"
       ,overwrite=TRUE)
    ;;;;
    %utl_rendx;

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|        _     _       _               _
      __ _  __| | __| |  ___| |__   ___  ___| |_
     / _` |/ _` |/ _` | / __| `_ \ / _ \/ _ \ __|
    | (_| | (_| | (_| | \__ \ | | |  __/  __/ |_
     \__,_|\__,_|\__,_| |___/_| |_|\___|\___|\__|

    */

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
     wb<-loadWorkbook("d:/xls/wantxl.xlsx")
     have<-read.xlsx(wb,"have")
     have
     addWorksheet(wb, "want")
     want<-sqldf('
      select
         *
        ,a1 * b1 +
         a2 * b2 +
         a3 * b3 +
         a4 * b4  as sumab
      from
         have
      ')
     print(want)
     writeData(wb,sheet="want",x=want)
     saveWorkbook(
         wb
        ,"d:/xls/wantxl.xlsx"
        ,overwrite=TRUE)
    ;;;;
    %utl_rendx;


     /**************************************************************************************************************************/
     /*                                                                                                                        */
     /* -----------------------+                                                                                               */
     /* | A1  | fx    |   a1   |                                                                                               */
     /* -----------------------------------------------------------------------------------------------                        */
     /* [_]|    A     |    B    |    C    |    E    |    F    |    G    |    H    |    I    |    J    |                        */
     /* -----------------------------------------------------------------------------------------------                        */
     /*  1 |   A1     |   A2    |   A3    |   A4    |   B1    |   B2    |   B3    |   B4    |  SUMAB  |                        */
     /* -- |----------+---------+---------+---------+---------+---------+---------+---------+---------+                        */
     /*  2 | 13       |15       |15       |11       |13       |15       |15       |11       |740      |                        */
     /* -- |----------+---------+---------+---------+---------+---------+---------+---------+---------+                        */
     /*  3 | 13       |14       |13       |11       |13       |14       |13       |11       |655      |                        */
     /* -- |----------+---------+---------+---------+---------+---------+---------+---------+---------+                        */
     /*  4 | 12       |11       |13       |15       |12       |11       |13       |15       |659      |                        */
     /* -- |----------+---------+---------+---------+---------+---------+---------+---------+---------+                        */
     /*  5 | 12       |12       |11       |13       |12       |12       |11       |13       |578      |                        */
     /* -- |----------+---------+---------+---------+---------+---------+---------+---------+---------+                        */
     /*  6 | 13       |13       |14       |12       |13       |13       |14       |12       |678      |                        */
     /* -- |----------+---------+---------+---------+---------+---------+---------+---------+---------+                        */
     /* [CLASS}                                                                                                                */
     /*                                                                                                                        */
     /*                                                                                                                        */
     /**************************************************************************************************************************/

    /*__                                                   __          _   __
     / /_    _ __   _ __ _____      _____ _   _ _ __ ___  / /__ ___/\_| |__\ \
    | `_ \  | `__| | `__/ _ \ \ /\ / / __| | | | `_ ` _ \| |/ _` \    / `_ \| |
    | (_) | | |    | | | (_) \ V  V /\__ \ |_| | | | | | | | (_| /_  _\ |_) | |
     \___/  |_|    |_|  \___/ \_/\_/ |___/\__,_|_| |_| |_| |\__,_| \/ |_.__/| |
                                                          \_\              /_/
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have<-as.matrix(
       read_sas("d:/sd1/have.sas7bdat"))
    want <- data.frame(sumab=
       rowSums(have[,1:4]*have[,5:8]))
    want<-cbind(want,have)
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* R                                  SAS                                                                                 */
    /*   sumab A1 A2 A3 A4 B1 B2 B3 B4    ROWNAMES    SUMAB    A1    A2    A3    A4    B1    B2    B3    B4                   */
    /*                                                                                                                        */
    /* 1   740 13 15 15 11 13 15 15 11        1        740     13    15    15    11    13    15    15    11                   */
    /* 2   655 13 14 13 11 13 14 13 11        2        655     13    14    13    11    13    14    13    11                   */
    /* 3   659 12 11 13 15 12 11 13 15        3        659     12    11    13    15    12    11    13    15                   */
    /* 4   578 12 12 11 13 12 12 11 13        4        578     12    12    11    13    12    12    11    13                   */
    /* 5   678 13 13 14 12 13 13 14 12        5        678     13    13    14    12    13    13    14    12                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
