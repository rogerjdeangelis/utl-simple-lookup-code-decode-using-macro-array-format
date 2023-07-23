# utl-simple-lookup-code-decode-using-macro-array-format
Simple lookup code decode using macro array format 
    %let pgm=utl-simple-lookup-code-decode-using-macro-array-format;


       Simple lookup code decode using macro array format

       github
       https://tinyurl.com/2p9ura6y
       https://github.com/rogerjdeangelis/utl-simple-lookup-code-decode-using-macro-array-format

       GIVEN CODES LOOKUP DECODES

       WH  =  White
       BL  =  Black or African American
       AS  =  Asian
       AA  =  American Indian or Alaska Native
       NH  =  Native Hawaiian or Other Pacific Islander
       MM  =  Multiple Categories Reported
       UN  =  Race Unknown

       Solutions

            1  wps/sas Macro lookup (fastest? macros mapping in memory)
            2. wps/sas array lookup
            3. wps/sas format lookup (binary seqrch?)
    /*                   _
    (_)_ __  _ __  _   _| |_ ___
    | | `_ \| `_ \| | | | __/ __|
    | | | | | |_) | |_| | |_\__ \
    |_|_| |_| .__/ \__,_|\__|___/
            |_|
    */
    options validvarname=upcase;
    libname sd1 "d:/sd1";

    /*----  CODES                                                            ----*/
    data sd1.code;
     informat ethcde $2.;
     input ethcde @@;
    cards4;
    WH BL AS AA NH MM UN
    ;;;;
    run;quit;

    /*---- DECODES                                                           ----*/
    data sd1.decode;
     informat ethcat $44.;
     input ethcat &;
    cards4;
    White
    Black or African American
    Asian
    American Indian or Alaska Native
    Native Hawaiian or Other Pacific Islander
    Multiple Categories Reported
    Race Unknown
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Codes and decodes match 1:1                                                                                                                      */
    /*                _                                                                                                       */
    /*   ___ ___   __| | ___  ___                                                                                             */
    /*  / __/ _ \ / _` |/ _ \/ __|                                                                                            */
    /* | (_| (_) | (_| |  __/\__ \                                                                                            */
    /*  \___\___/ \__,_|\___||___/                                                                                            */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* SD1.CODE total obs=6 19MAY2023:09:02:53                                                                                */
    /*                                                                                                                        */
    /* Obs    AGECDE                                                                                                          */
    /*                                                                                                                        */
    /*  1       WH                                                                                                            */
    /*  2       BL                                                                                                            */
    /*  3       AS                                                                                                            */
    /*  4       AA                                                                                                            */
    /*  5       NH                                                                                                            */
    /*  6       MM                                                                                                            */
    /*  7       UN                                                                                                            */
    /*      _                    _                                                                                            */
    /*   __| | ___  ___ ___   __| | ___  ___                                                                                  */
    /*  / _` |/ _ \/ __/ _ \ / _` |/ _ \/ __|                                                                                 */
    /* | (_| |  __/ (_| (_) | (_| |  __/\__ \                                                                                 */
    /*  \__,_|\___|\___\___/ \__,_|\___||___/                                                                                 */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* SD1.DECODE total obs=7 19MAY2023:15:42:14                                                                              */
    /*                                                                                                                        */
    /* Obs    ETHCAT                                                                                                          */
    /*                                                                                                                        */
    /*  1     White                                                                                                           */
    /*  2     Black or African American                                                                                       */
    /*  3     Asian                                                                                                           */
    /*  4     American Indian or Alaska Native                                                                                */
    /*  5     Native Hawaiian or Other Pacific Islander                                                                       */
    /*  6     Multiple Categories Reported                                                                                    */
    /*  7     Race Unknown                                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                         __                                               _             _
    / | __      ___ __  ___   / /__  __ _ ___   _ __ ___   __ _  ___ _ __ ___  | | ___   ___ | | ___   _ _ __
    | | \ \ /\ / / `_ \/ __| / / __|/ _` / __| | `_ ` _ \ / _` |/ __| `__/ _ \ | |/ _ \ / _ \| |/ / | | | `_ \
    | |  \ V  V /| |_) \__ \/ /\__ \ (_| \__ \ | | | | | | (_| | (__| | | (_) || | (_) | (_) |   <| |_| | |_) |
    |_|   \_/\_/ | .__/|___/_/ |___/\__,_|___/ |_| |_| |_|\__,_|\___|_|  \___/ |_|\___/ \___/|_|\_\\__,_| .__/
                 |_|                                                                                    |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %symdel WH BL AS AA NH MM UN / nowarn;

    %let WH  =  White                                     ;
    %let BL  =  Black or African American                 ;
    %let AS  =  Asian                                     ;
    %let AA  =  American Indian or Alaska Native          ;
    %let NH  =  Native Hawaiian or Other Pacific Islander ;
    %let MM  =  Multiple Categories Reported              ;
    %let UN  =  Race Unknown                              ;

    libname sd1 "d:/sd1";

    data sd1.want;
      length ethcat $44;
      set sd1.dem;
      ethcat=symgetc(ethcde);
    run;quit;

    /*
    __      ___ __  ___
    \ \ /\ / / `_ \/ __|
     \ V  V /| |_) \__ \
      \_/\_/ | .__/|___/
             |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('

    %let WH  =  White                                     ;
    %let BL  =  Black or African American                 ;
    %let AS  =  Asian                                     ;
    %let AA  =  American Indian or Alaska Native          ;
    %let NH  =  Native Hawaiian or Other Pacific Islander ;
    %let MM  =  Multiple Categories Reported              ;
    %let UN  =  Race Unknown                              :

    run;quit;

    libname sd1 "d:/sd1";
    data sd1.want;
      length ethcat $44;
      set sd1.code;
      ethcat=symget(ethcde);
      put ethcde=;
    run;quit;

    proc print data=sd1.want;
    run;quit;

    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs                     ETHCAT                      ETHCDE                                                             */
    /*                                                                                                                        */
    /*  1     White                                          WH                                                               */
    /*  2     Black or African American                      BL                                                               */
    /*  3     Asian                                          AS                                                               */
    /*  4     American Indian or Alaska Native               AA                                                               */
    /*  5     Native Hawaiian or Other Pacific Islander      NH                                                               */
    /*  6     Multiple Categories Reported                   MM                                                               */
    /*  7     Race Unknown                                   UN                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                          __                __                            _     _             _
    |___ \  __      ___ __  ___   / /__  __ _ ___   / _| ___  _ __ _ __ ___   __ _| |_  | | ___   ___ | | ___   _ _ __
      __) | \ \ /\ / / `_ \/ __| / / __|/ _` / __| | |_ / _ \| `__| `_ ` _ \ / _` | __| | |/ _ \ / _ \| |/ / | | | `_ \
     / __/   \ V  V /| |_) \__ \/ /\__ \ (_| \__ \ |  _| (_) | |  | | | | | | (_| | |_  | | (_) | (_) |   <| |_| | |_) |
    |_____|   \_/\_/ | .__/|___/_/ |___/\__,_|___/ |_|  \___/|_|  |_| |_| |_|\__,_|\__| |_|\___/ \___/|_|\_\\__,_| .__/
                     |_|                                                                                         |_|

    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    proc format ;
      value $ethcde2ethcat
        "WH"    =  "White"
        "BL"    =  "Black or African American"
        "AS"    =  "Asian"
        "AA"    =  "American Indian or Alaska Native"
        "NH"    =  "Native Hawaiian or Other Pacific Islander"
        "MM"    =  "Multiple Categories Reported"
        "UN"    =  "Race Unknown" ;
    run;quit;

    data want;
      set sd1.code;
      ethcat=put(ethcde,$ethcde2ethcat.);
    run;quit;

    proc print;
    run;quit;

    /*
    __      ___ __  ___
    \ \ /\ / / `_ \/ __|
     \ V  V /| |_) \__ \
      \_/\_/ | .__/|___/
             |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('

    libname sd1 "d:/sd1";

    proc format ;
      value $ethcde2ethcat
        "WH"    =  "White"
        "BL"    =  "Black or African American"
        "AS"    =  "Asian"
        "AA"    =  "American Indian or Alaska Native"
        "NH"    =  "Native Hawaiian or Other Pacific Islander"
        "MM"    =  "Multiple Categories Reported"
        "UN"    =  "Race Unknown"
    ;run;quit;

    data sd1.want;
      set sd1.code;
      ethcat=put(ethcde,$ethcde2ethcat.);
    run;quit;

    proc print data=sd1.want;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs    ETHCDE                     ETHCAT                                                                               */
    /*                                                                                                                        */
    /*  1       WH      White                                                                                                 */
    /*  2       BL      Black or African American                                                                             */
    /*  3       AS      Asian                                                                                                 */
    /*  4       AA      American Indian or Alaska Native                                                                      */
    /*  5       NH      Native Hawaiian or Other Pacific Islander                                                             */
    /*  6       MM      Multiple Categories Reported                                                                          */
    /*  7       UN      Race Unknown                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                         __                                            _             _
    |___ /  __      ___ __  ___   / /__  __ _ ___    __ _ _ __ _ __ __ _ _   _  | | ___   ___ | | ___   _ _ __
      |_ \  \ \ /\ / / `_ \/ __| / / __|/ _` / __|  / _` | `__| `__/ _` | | | | | |/ _ \ / _ \| |/ / | | | `_ \
     ___) |  \ V  V /| |_) \__ \/ /\__ \ (_| \__ \ | (_| | |  | | | (_| | |_| | | | (_) | (_) |   <| |_| | |_) |
    |____/    \_/\_/ | .__/|___/_/ |___/\__,_|___/  \__,_|_|  |_|  \__,_|\__, | |_|\___/ \___/|_|\_\\__,_| .__/
                     |_|                                                 |___/                           |_|

    */


    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    data sd1.want;

       set sd1.code;

       array ethidx[7] $2  _temporary_ ('WH' 'BL' 'AS' 'AA' 'NH' 'MM' 'UN');
       array ethnam[7] $44 _temporary_ (
             'White'
             'Black or African American'
             'Asian'
             'American Indian or Alaska Native'
             'Native Hawaiian or Other Pacific Islander'
             'Multiple Categories Reported'
             'Race Unknown' );

       ethcat=ethnam[whichc(ethcde, of ethidx[*])];

    run;quit;

    proc print data=sd1.want;
    run;quit;

    /*
    __      ___ __  ___
    \ \ /\ / / `_ \/ __|
     \ V  V /| |_) \__ \
      \_/\_/ | .__/|___/
             |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('

    libname sd1 "d:/sd1";

    data sd1.want;

       set sd1.code;

       array ethidx[7] $2  _temporary_ ("WH" "BL" "AS" "AA" "NH" "MM" "UN");
       array ethnam[7] $44 _temporary_ (
             "White"
             "Black or African American"
             "Asian"
             "American Indian or Alaska Native"
             "Native Hawaiian or Other Pacific Islander"
             "Multiple Categories Reported"
             "Race Unknown" );

       ethcat=ethnam[whichc(ethcde, of ethidx[*])];

    run;quit;

    proc print data=sd1.want;
    run;quit;

    ');


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs    ETHCDE                     ETHCAT                                                                               */
    /*                                                                                                                        */
    /*  1       WH      White                                                                                                 */
    /*  2       BL      Black or African American                                                                             */
    /*  3       AS      Asian                                                                                                 */
    /*  4       AA      American Indian or Alaska Native                                                                      */
    /*  5       NH      Native Hawaiian or Other Pacific Islander                                                             */
    /*  6       MM      Multiple Categories Reported                                                                          */
    /*  7       UN      Race Unknown                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

