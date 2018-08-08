# utl_five_programs_to_subset_and_appending_ten_identical_10gb_tables
Five programs to subset and appending ten identical 10gb tables.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Five programs to subset and append ten identical 10gb tables

    github
    https://tinyurl.com/ybnop9c3
    https://github.com/rogerjdeangelis/utl_five_programs_to_subset_and_appending_ten_identical_10gb_tables

     It does not matter how you do this, this requires serial I/O bound processing.
     Difficult to parallelize.

     Proc append takes seems to have a slight edge.
     SQL union wan not competitive.
     Slight advantage using a separate drive for output table.

     BENCHMARKS
     ----------
           Minutes
          /Seconds

        1. 12:39    One datastep and one where
        2. 25:45    SQL 10 outer unions
        3. 11:11    One datastep with one if statement (surprise)
        4. 12:11    One datastep with 10 separate where clauses
        5. 10:36    Datastep then 8 proc appends

    SAS Forum
    https://tinyurl.com/ydd5azpn
    https://communities.sas.com/t5/Base-SAS-Programming/Subsetting-from-multiple-dataset/m-p/484864


    INPUT
    =====

        WORK.HAV1 has 209,000,000  10gb
        WORK.HAV2 has 209,000,000  10gb
        WORK.HAV3 has 209,000,000  10gb
        WORK.HAV4 has 209,000,000  10gb
        WORK.HAV5 has 209,000,000  10gb
        WORK.HAV6 has 209,000,000  10gb
        WORK.HAV7 has 209,000,000  10gb
        WORK.HAV8 has 209,000,000  10gb
        WORK.HAV9 has 209,000,000  10gb


      TEN OF THESE
      ------------

      WORK.HAV1 total obs=209,000,000

                Obs     Name     Sex    Age    Height    Weight           rec

                  1    Alfred     M      14      69       112.5             1
                  2    Alfred     M      14      69       112.5             2
                  3    Alfred     M      14      69       112.5             3
                  4    Alfred     M      14      69       112.5             4
              ....
          209000000   WIlliam     M      15      89       119.5     209000000

      RULE
      ----

        Append all 10 where sex='F'


    PROCESSES
    =========

     1. 12:39    One datastep and one where
     --------------------------------------

       2794  data f.want;
       2795    set hav1 hav2 hav3 hav4 hav5 hav6 hav7 hav8 hav9 hav10;
       2796    where sex='F';
       2797  run;

       NOTE: There were 99000000 observations read from the data set WORK.HAV1.
       NOTE: There were 99000000 observations read from the data set WORK.HAV2.
       NOTE: There were 99000000 observations read from the data set WORK.HAV3.
       NOTE: There were 99000000 observations read from the data set WORK.HAV4.
       NOTE: There were 99000000 observations read from the data set WORK.HAV5.
       NOTE: There were 99000000 observations read from the data set WORK.HAV6.
       NOTE: There were 99000000 observations read from the data set WORK.HAV7.
       NOTE: There were 99000000 observations read from the data set WORK.HAV8.
       NOTE: There were 99000000 observations read from the data set WORK.HAV9.
       NOTE: There were 99000000 observations read from the data set WORK.HAV10.
       NOTE: The data set WORK.WANT has 990000000 observations and 6 variables.
       NOTE: DATA statement used (Total process time):
             real time           12:39.76
             user cpu time       3:27.15

     2. 25:45    SQL 10 outer unions
     -------------------------------

       proc sql;
            create
              table f.want as
            select * from hav1 where sex='F' outer union corr
            select * from hav2 where sex='F' outer union corr
            select * from hav3 where sex='F' outer union corr
            select * from hav4 where sex='F' outer union corr
            select * from hav5 where sex='F' outer union corr
            select * from hav6 where sex='F' outer union corr
            select * from hav7 where sex='F' outer union corr
            select * from hav8 where sex='F' outer union corr
            select * from hav9 where sex='F' outer union corr
            select * from hav10  where sex='F'
        ;
       quit;

       2863     quit;
       NOTE: PROCEDURE SQL used (Total process time):
             real time           25:48.46
             user cpu time       16:46.45
             system cpu time     8:32.52


     3. 11:11    One datastep and one if statement (surprise)
     --------------------------------------------------------

       2868      data f.want_if;
       2869        set hav1 hav2 hav3 hav4 hav5 hav6 hav7 hav8 hav9 hav10;
       2870        if sex='F';
       2871      run;

       NOTE: There were 209000000 observations read from the data set WORK.HAV1.
       NOTE: There were 209000000 observations read from the data set WORK.HAV2.
       NOTE: There were 209000000 observations read from the data set WORK.HAV3.
       NOTE: There were 209000000 observations read from the data set WORK.HAV4.
       NOTE: There were 209000000 observations read from the data set WORK.HAV5.
       NOTE: There were 209000000 observations read from the data set WORK.HAV6.
       NOTE: There were 209000000 observations read from the data set WORK.HAV7.
       NOTE: There were 209000000 observations read from the data set WORK.HAV8.
       NOTE: There were 209000000 observations read from the data set WORK.HAV9.
       NOTE: There were 209000000 observations read from the data set WORK.HAV10.
       NOTE: The data set F.WANT_IF has 990000000 observations and 6 variables.
       NOTE: DATA statement used (Total process time):
             real time           11:10.83
             user cpu time       2:35.53
             system cpu time     8:13.77
             memory              14701.34k
             OS Memory           32704.00k
             Timestamp           08/07/2018 07:41:54 PM
             Step Count                        415  Switch Count  11

       2871!         quit;


     4. 12:11    One datastep with 10 separate where clauses
     -------------------------------------------------------

       2872      data f.want;
       2873      set
       2874      hav1(where=(sex='F'))
       2875      hav2(where=(sex='F'))
       2876      hav3(where=(sex='F'))
       2877      hav4(where=(sex='F'))
       2878      hav5(where=(sex='F'))
       2879      hav6(where=(sex='F'))
       2880      hav7(where=(sex='F'))
       2881      hav8(where=(sex='F'))
       2882      hav9(where=(sex='F'))
       2883      hav10(where=(sex='F'))
       2884      ;
       2885      run;

       2885!         quit;
       NOTE: There were 99000000 observations read from the data set WORK.HAV1.
       NOTE: There were 99000000 observations read from the data set WORK.HAV2.
       NOTE: There were 99000000 observations read from the data set WORK.HAV3.
       NOTE: There were 99000000 observations read from the data set WORK.HAV4.
       NOTE: There were 99000000 observations read from the data set WORK.HAV5.
       NOTE: There were 99000000 observations read from the data set WORK.HAV6.
       NOTE: There were 99000000 observations read from the data set WORK.HAV7.
       NOTE: There were 99000000 observations read from the data set WORK.HAV8.
       NOTE: There were 99000000 observations read from the data set WORK.HAV9.
       NOTE: There were 99000000 observations read from the data set WORK.HAV10.
       NOTE: The data set F.WANT has 990000000 observations and 6 variables.
       NOTE: DATA statement used (Total process time):
             real time           12:11.57
             user cpu time       3:07.68



     5. 10:36    Datastep then 8 proc appends
     ----------------------------------------

       libname f "f:/wrk";

       data f.havAll;
         set hav1 hav2;
         where sex='F';
       run;quit;

       real time           2:28.26

       proc append data=hav3(where=(sex='F')) base=f.hav12;run;quit;
       proc append data=hav4(where=(sex='F')) base=f.hav12;run;quit;
       proc append data=hav5(where=(sex='F')) base=f.hav12;run;quit;
       proc append data=hav6(where=(sex='F')) base=f.hav12;run;quit;
       proc append data=hav7(where=(sex='F')) base=f.hav12;run;quit;
       proc append data=hav8(where=(sex='F')) base=f.hav12;run;quit;
       proc append data=hav9(where=(sex='F')) base=f.hav12;run;quit;
       proc append data=hav10(where=(sex='F')) base=f.hav12;run;quit;

       * time for one of the 8 appends;

       NOTE: 99000000 observations added. (9 females to each 10 males)

       NOTE: The data set F.HAV12 has 297000000 observations and 6 variables.
       NOTE: PROCEDURE APPEND used (Total process time):
             real time           1:00.53

           TOTAL TIME
           ----------

              2:28
              8.08   8  appends
              ====
             10.36


    OUTPUT
    ======

     Data Set Name        F.WANT                        Observations          990000000
     Created              08/07/2018 19:45:06           Observation Length    48


            Engine/Host Dependent Information

     Data Set Page Size          65536
     Number of Data Set Pages    727407

     Filename                    f:\wrk\want.sas7bdat
     Release Created             9.0401M2
     Host Created                X64_7PRO


     Alphabetic List of Variables and Attributes

     #    Variable    Type    Len

     3    Age         Num       8
     4    Height      Num       8
     1    Name        Char      8
     2    Sex         Char      1
     5    Weight      Num       8
     6    rec         Num       8


    F.WANT total obs=990,000,000

         Name     Sex    Age    Height    Weight     REC

         Alice     F      13     56.5       84         1
         Alice     F      13     56.5       84         2
         Alice     F      13     56.5       84         3
        ....
         Mary      F      15     66.5      112  989999998
         Mary      F      15     66.5      112  989999999
         Mary      F      15     66.5      112  990000000




         sashelp.class



    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    * about 10 gb each;
    data hav1 hav2 hav3 hav4 hav5 hav6 hav7 hav8 hav9 hav10;

      set sashelp.class;
      do rec=1 to 11000000;
        output;
      end;

    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process



