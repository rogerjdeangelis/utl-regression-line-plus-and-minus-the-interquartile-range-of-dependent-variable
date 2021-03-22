# utl-regression-line-plus-and-minus-the-interquartile-range-of-dependent-variable
Regression line plus and minus the interquartile range of dependent variable
   Median polish might be more appropriate?

    Regression line plus and minus the interquartile range of dependent variable;

                 7.5              8.0              8.5              9.0              9.5
         X Axis  -+----------------+----------------+----------------+----------------+--
                 |                                                                      |
               9 +                  Inter Quartile Range  .                             + 9
                 |   X Q1    X Q2   IQR                                                 |
                 |    4.3     5.7   1.4                                               U |
                 |                                                                 UUU  |
                 |   Regression line plus and minus the interquartile           UUUU    |
               8 +   range of dependent variable                .           UUU         + 8
                 |                                                       UUU            |
                 |                                       ..          UUUU               |
                 |                        .       .   .         . UUUU                  |  _ _
                 |                     .   .                   UUUU                   # |   |
               7 +     .           .        . ..       .    UU.U ..   .            ###  + 7 |
                 |      .     .          ..  .     .     UUUU  .  .            ####     |   |  IQR
                 |            .       .    . .        U.UU    .    .  .     ####        |   |_ 1.4
                 |               .    .    .   . . U..U.. .. .   ..      ###        .   |   |
                 |        . .      ..     .    ...U..    .  ..  .  . ####.              |   |
               6 + .     .     .     . ..   UUU. .... .   .. .. . ####..     .          + 6 |
                 |      .      . .       .U.... . .. ..   .. . ####                   L |  _|_
                 |             . . ....UU..         .... ...#### . ..       .   .  LLL. |
                 |    . .      .   UU..    ..  ...   ....####. . .   ..  . ....LLLL . . |
                 |     .        .UU..  .. ....... . ..#### .. .... .. .    ..LL.        |
               5 +.       .  UUUU   .. . ..   .. ..####.. . .  . ..      LLL. ..   .    + 5
                 | .   .  U U....   .. .   .. ..####.....  .  . .  . .LLL          .    |
                 |   . UUUU . .  .   .  .  ..####........   .. ... LLL.  .     .        |
                 |   UU          . . ..  #### . .. .     ..  ..LL.L .      .            |
          _ _    |UU ..      .     .. ####    . .  . .. . . LL..    ...   ....    . ..  |
           |   4 +            .   .#### .   .  . .  .  . .LLL .   ..   ...      .     . + 4
           |     |              ####        ...    .  LLLL.   .     .  .  .    ...      |
       IQR |     |        .  ####     ..   .       L.L. .... . .  .        . . .        |
       1.4_|     |        # ##   .   ..  .      L.LL. .  ..                             |
           |     |     ####                  LLLL  .  ..         . .                    |
           |   3 +   ###      . .         LLL     ..  . .           .     .             + 3
           |     |##                  LLLL                                  .           |
          _|_    |               . LLLL                 .                               |
                 |              LLLL                     .                              |
                 |           LLLL                                             .         |
               2 +        L LL                                                          + 2
                 |     LLLL                        .                                    |
                 |   LLL                                                                |
                 |LL                                                                    |
                 |                                                                      |
               1 +                                                                      + 1
                 |                                                                      |
                 -+----------------+----------------+----------------+----------------+--
                 7.5              8.0              8.5              9.0              9.5

                                             Y Axis
    GitHub
    https://tinyurl.com/3ubxrmba
    https://github.com/rogerjdeangelis/utl-regression-line-plus-and-minus-the-interquartile-range-of-dependent-variable

    R Plot

    GitHub
    https://tinyurl.com/9bwbh5vt
    https://github.com/rogerjdeangelis/utl-regression-line-plus-and-minus-the-interquartile-range-of-dependent-variable/blob/main/ablines.pdf


         Two Solutions
               a. R
               b. SAS

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;


    options validvarname=upcase;

    libname sd1 "d:/sd1";

    data sd1.have;
     call streaminit(35358);
     do _n_=1 to 500;
        y=rand('normal',1.5,.5) + 7;
        x=rand('normal')+ 5;
        output;
     end;
    run;quit;


    SD1.HAVE total obs=500

    Obs       Y          X

      1    8.63856    5.59046
      2    9.94045    5.86386
      3    8.36453    4.66362
     ...
    498    9.50128    5.68167
    499    9.00540    4.24383
    500    8.10709    4.87397

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|        ____
      __ _    |  _ \
     / _` |   | |_) |
    | (_| |_  |  _ <
     \__,_(_) |_| \_\

    ;

    %utlfkil(d:/pdf/ablines.pdf);
    %utlfkil(d:/sd1/want_r.sas7bdat);

    %utl_submit_r64(%tslit(
    library(haven);
    library(RJDBC);
    have<-read_sas("d:/sd1/have.sas7bdat");
    attach(have);
    Xq <- quantile(X);
    Yq <- quantile(Y);
    df <- data.frame(X, Y);
    pdf(file="d:/pdf/ablines.pdf");
    plot(X~Y, df, pch=20);
    diag <- lm(Xq[c(2, 4)]~Yq[c(2, 4)]);
    abline(diag);
    b <- coef(diag)[2];
    a1 <- Xq[4] - b * Yq[2];
    a2 <- Xq[2] - b * Yq[4];
    abline(a1, b);
    abline(a2, b);
    pdf();
    res1 <- X - (a1 + b * Y);
    res2 <- (a2 + b * Y) - X;
    idx <- ifelse(res1 > 0, 3, ifelse(res2 > 0, 2, 1));
    want_r<-cbind(X,Y,idx);
    drv<- JDBC("com.dullesopen.jdbc.Driver","d:/carolina/carolina-jdbc-2.4.3.jar");
    conn <- dbConnect(drv, "jdbc:carolina:bulk:libnames=(dir='d:/sd1')", "", "");
    rc<- dbWriteTable(conn,"want_r",want_r);
    ));

    *_
    | |__     ___  __ _ ___
    | '_ \   / __|/ _` / __|
    | |_) |  \__ \ (_| \__ \
    |_.__(_) |___/\__,_|___/

    ;

    * get the quartiles;

    %symdel e1 e2 ymin ymax x_q1  x_q3  y_q1  y_q3;

    proc datasets lib=work nolist;
      delete havXpo havNrm havCof want;
    run;quit;

    libname sd1 "d:/sd1";

    proc means data=sd1.have  missing stackodsoutput;
    var x y;
    output out=havXpo(drop=_type_ _freq_) q1= q3= / autoname;
    run;

    data havNrm;
      set havXpo;
      x=X_Q1; y=Y_Q1 ;call symputx('X_Q1',X_Q1); call symputx('Y_Q1',Y_Q1);output;
      x=X_Q3; y=Y_Q3 ;call symputx('X_Q3',X_Q3); call symputx('Y_Q3',Y_Q3);output;
      keep x y;
    run;quit;

    /*
    Up to 40 obs from HAVXPO total obs=1

    Obs      X_Q1       Y_Q1       X_Q3       Y_Q3

     1     4.28203    8.17777    5.68680    8.82110

     %put  &=X_Q1;
     %put  &=X_Q3;
     %put  &=Y_Q1;
     %put  &=Y_Q3;

    X_Q1=4.2820273061
    X_Q3=5.6868004885
    Y_Q1=8.1777679921
    Y_Q3=8.8211004131

    */

    * get slope and intercept;
    proc reg data=havNrm;
    ods output ParameterEstimates=havCof;
    model x=y;
    run;quit;

    proc sql;
       select
          estimate into :e1-
       from
          havCof
    ;quit;

    * get max and min x and y;
    proc sql;
       select
          min(y)
         ,max(y)
       into
          :yMin
         ,:yMax
       from
          have
    ;quit;

    * Regression line plus and minus the interquatile range;
    data want;
        set sd1.have;
        xreg  =&e2*y +&e1;
        q31    =&e2*y - (&e2*&y_Q3 - &x_Q1) ;
        q13    =&e2*y + (&x_Q3 - &e2*&y_Q1) ;
        UprAdd=q13-xreg;
        LwrLes=xreg-q31;
        select;
           when ( x > q13 ) idx="Upper ";
           when ( x < q31 ) idx="LOWER ";
           otherwise        Idx="INSIDE";
        END;
        output;
    run;quit;

    options ps=55 ls=80;
    proc plot data=want;
      plot  xreg*y="#"  x*y="." q31*y="L" q13*y="U" / overlay box
      haxis= 7.5 to 9.5 by .5 vaxis=1 to 9;
    run;quit;

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
               ___  |_|
      __ _    |  _ \
     / _` |   | |_) |
    | (_| |_  |  _ <
     \__,_(_) |_| \_\
    ;

    Up to 40 obs from SD1.WANT_R total obs=500

    Obs       X          Y       IDX

      1    5.59046    8.63856     1
      2    5.86386    9.94045     2
      3    4.66362    8.36453     1
      4    5.67376    8.65808     1
      5    4.74283    8.63023     1
      6    5.07118    8.31332     1

    ...

    98    5.68167    9.50128     2
    99    4.24383    9.00540     2
    00    4.87397    8.10709     1



                                    Cumulative    Cumulative
    IDX    Frequency     Percent     Frequency      Percent
    --------------------------------------------------------
      1         312       62.40           312        62.40
      2          93       18.60           405        81.00
      3          95       19.00           500       100.00

    *_
    | |__     ___  __ _ ___
    | '_ \   / __|/ _` / __|
    | |_) |  \__ \ (_| \__ \
    |_.__(_) |___/\__,_|___/

    ;

    Up to 40 obs from want total obs=500

    Obs     Y        X       XREG      Q31      Q13     UPRADD   LWRLES   IDX

      1  8.63856  5.59046  5.28823   3.88344  6.69298  1.40476  1.40479  INSIDE
      2  9.94045  5.86386  8.13103   6.72624  9.53579  1.40476  1.40479  LOWER
      3  8.36453  4.66362  4.68985   3.28506  6.09461  1.40476  1.40479  INSIDE
    ...
    497  8.29457  5.23548  4.53709   3.13230  5.94184  1.40476  1.40479  INSIDE
    498  9.50128  5.68167  7.17204   5.76726  8.57680  1.40476  1.40479  LOWER
    499  9.00540  4.24383  6.08924   4.68445  7.49400  1.40476  1.40479  LOWER
    500  8.10709  4.87397  4.12770   2.72291  5.53246  1.40476  1.40479  INSIDE


                                       Cumulative    Cumulative
    IDX       Frequency     Percent     Frequency      Percent
    -----------------------------------------------------------
    INSIDE         313       62.60           313        62.60
    LOWER           93       18.60           406        81.20
    Upper           94       18.80           500       100.00


    options ps=55 ls=80;
    proc plot data=want;
      plot  xreg*y="#"  x*y="." q31*y="L" q13*y="U" / overlay box
      haxis= 7.5 to 9.5 by .5 vaxis=1 to 9;
    run;quit;

    * you will need to use a text editor


    Regression line plus and minus the interquartile range of dependent variable;

                 7.5              8.0              8.5              9.0              9.5
         X Axis  -+----------------+----------------+----------------+----------------+--
                 |                                                                      |
               9 +                                     .  .                             + 9
                 |   X Q1    X Q2   IQR                                                 |
                 |    4.3     5.7   1.4                                               U |
                 |                                                                 UUU  |
                 |   Regression line plus and minus the interquartile           UUUU    |
               8 +   range of dependent variable                .           UUU         + 8
                 |                                                       UUU            |
                 |                                       ..          UUUU               |
                 |                        .       .   .         . UUUU                  |  _ _
                 |                     .   .                   UUUU                   # |   |
               7 +     .           .        . ..       .    UU.U ..   .            ###  + 7 |
                 |      .     .          ..  .     .     UUUU  .  .            ####     |   |  IQR
                 |            .       .    . .        U.UU    .    .  .     ####        |   |_ 1.4
                 |               .    .    .   . . U..U.. .. .   ..      ###        .   |   |
                 |        . .      ..     .    ...U..    .  ..  .  . ####.              |   |
               6 + .     .     .     . ..   UUU. .... .   .. .. . ####..     .          + 6 |
                 |      .      . .       .U.... . .. ..   .. . ####                   L |  _|_
                 |             . . ....UU..         .... ...#### . ..       .   .  LLL. |
                 |    . .      .   UU..    ..  ...   ....####. . .   ..  . ....LLLL . . |
                 |     .        .UU..  .. ....... . ..#### .. .... .. .    ..LL.        |
               5 +.       .  UUUU   .. . ..   .. ..####.. . .  . ..      LLL. ..   .    + 5
                 | .   .  U U....   .. .   .. ..####.....  .  . .  . .LLL          .    |
                 |   . UUUU . .  .   .  .  ..####........   .. ... LLL.  .     .        |
                 |   UU          . . ..  #### . .. .     ..  ..LL.L .      .            |
          _ _    |UU ..      .     .. ####    . .  . .. . . LL..    ...   ....    . ..  |
           |   4 +            .   .#### .   .  . .  .  . .LLL .   ..   ...      .     . + 4
           |     |              ####        ...    .  LLLL.   .     .  .  .    ...      |
       IQR |     |        .  ####     ..   .       L.L. .... . .  .        . . .        |
       1.4_|     |        # ##   .   ..  .      L.LL. .  ..                             |
           |     |     ####                  LLLL  .  ..         . .                    |
           |   3 +   ###      . .         LLL     ..  . .           .     .             + 3
           |     |##                  LLLL                                  .           |
          _|_    |               . LLLL                 .                               |
                 |              LLLL                     .                              |
                 |           LLLL                                             .         |
               2 +        L LL                                                          + 2
                 |     LLLL                        .                                    |
                 |   LLL                                                                |
                 |LL                                                                    |
                 |                                                                      |
               1 +                                                                      + 1
                 |                                                                      |
                 -+----------------+----------------+----------------+----------------+--
                 7.5              8.0              8.5              9.0              9.5

                                           Y Axis

