# utl_SQL-odd-behavior-and-concatenating-mutiple-datasets-in-the-SQL-from-clause
SQL odd behavior and concatenating mutiple datasets in the SQL from clause

    THIS IS NOT ODD BEHAVIOR THE SECOND DATA SET IS NOT A DATASET BUT AN UNUSED ALIAS. I ALWAYS USE AN "AS" AND FORGOT ABOUT THIS SYNTAX.
    THANKS TO TIM BERRYHILL FOR POINTING THIS OUT.

    %let pgm=utl-SQL-odd-behavior-and-concatenating-multiple-datasets-in-the-SQL-from-clause;

    SQL odd behavior and concatenating multiple datasets in the SQL from clause

    github
    https://tinyurl.com/zsbffayp
    https://github.com/rogerjdeangelis/utl_SQL-odd-behavior-and-concatenating-mutiple-datasets-in-the-SQL-from-clause

    The following code produces no warnings, errors or notes.
    SAS may be trying to do some kind of join.

    There are other ways to do this, ie the sql UNION clause.

    data class;
      set sashelp.class;
    run;quit;

    data classfit;
      set sashelp.classfit;
      output;
      if _n_=1 then do; name="XYZZ";output; end;
    run;quit;

    * NOTE THERE ARE TWO DATASETS IN THE FROM CLAUSE SAS SEEMS TO IGNORE THE SECOND ONE
      AND DOES NOT COUNT 20 NAMES;

    proc sql;
        create
           table addnamr as
        select
           count (distinct name) as students
        from
           CLASS CLASSFIT;
    quit;

    Up to 40 obs WORK.ADDNAMR total obs=1 18NOV2021:07:52:17

    Obs    STUDENTS

     1        19

    * Here is one way to do the concatenation with error checking


    * THIS CONCATENATES THE TWO DATASETS WITH ERROR CHECKIING AND HAS THE CORRECT COUNT;

    %symdel cc / nowarn;

    proc sql;/*n=20 dos not add XYZZ NAME=XYZZ */
        create
            table counts as
        select
            count(distinct name) as allobs
           ,case
              when ("&cc"="0") then "CONCATENATION SUCCESSFUL"
              else "CONATENATION FAILED"
            end as status
        from
           %let rc=%sysfunc(dosubl('data both; set CLASS CLASSFIT;run;quit;%let cc=&syserr;'));
           %if &cc=0 %then %do; %str(both) ; %end;
    quit;

    Up to 40 obs WORK.COUNTS total obs=1 18NOV2021:07:55:11

    Obs    ALLOBS             STATUS

     1       20      CONTATENATION SUCCESSFUL


    %let pgm=utl_SQL-odd-behavior-and-concatenating-mutiple-datasets-in-the-SQL-from-clause;

    SQL odd behavior and concatenating multiple datasets in the SQL from clause

    github
    https://tinyurl.com/zsbffayp
    https://github.com/rogerjdeangelis/utl_SQL-odd-behavior-and-concatenating-mutiple-datasets-in-the-SQL-from-clause

    The following code produces no warnings, errors or notes.
    SAS may be trying to do some kind of 'NATURAL' join.

    There are other ways to do this, ie the sql UNION clause.

    data class;
      set sashelp.class;
    run;quit;

    data classfit;
      set sashelp.classfit;
      output;
      if _n_=1 then do; name="XYZZ";output; end;
    run;quit;

    * NOTE THERE ARE TWO DATASETS IN THE FROM CLAUSE SAS SEEMS TO IGNORE THE SECOND ONE
      AND DOES NOT COUNT 20 NAMES;

    proc sql;
        create
           table addnamr as
        select
           count (distinct name) as students
        from
           CLASS CLASSFIT;
    quit;

    Up to 40 obs WORK.ADDNAMR total obs=1 18NOV2021:07:52:17

    Obs    STUDENTS

     1        19

    * Here is one way to do the condatenation with error checking


    * THIS CONCATENATES THE TWO DATASETS WITH ERROR CHECKIING AND HAS THE CORRECT COUNT;

    %symdel cc / nowarn;

    proc sql;/*n=20 dos not add XYZZ NAME=XYZZ */
        create
            table counts as
        select
            count(distinct name) as allobs
           ,case
              when ("&cc"="0") then "CONTCATENATION SUCCESSFUL"
              else "CONCATENATION FAILED"
            end as status
        from
           %let rc=%sysfunc(dosubl('data both; set CLASS CLASSFIT;run;quit;%let cc=&syserr;'));
           %if &cc=0 %then %do; %str(both) ; %end;
    quit;

    Up to 40 obs WORK.COUNTS total obs=1 18NOV2021:07:55:11

    Obs    ALLOBS             STATUS

     1       20      CONTCATENATION SUCCESSFUL
