# utl-add-big-N-counts-to-your-column-headers
Add big N counts to your column headers 
    SAS Forum: Add big N counts to your column headers                                                                            
                                                                                                                                  
    github                                                                                                                        
    https://tinyurl.com/ufmq29e                                                                                                   
    https://github.com/rogerjdeangelis/utl-add-big-N-counts-to-your-column-headers                                                
                                                                                                                                  
    SAS forum (related to)                                                                                                        
    https://tinyurl.com/w3wcb9j                                                                                                   
    https://communities.sas.com/t5/SAS-Programming/How-Can-I-Generate-Macro-Variables-in-proc-SQL-appropriately/m-p/609853        
                                                                                                                                  
    *_                   _                                                                                                        
    (_)_ __  _ __  _   _| |_                                                                                                      
    | | '_ \| '_ \| | | | __|                                                                                                     
    | | | | | |_) | |_| | |_                                                                                                      
    |_|_| |_| .__/ \__,_|\__|                                                                                                     
            |_|                                                                                                                   
    ;                                                                                                                             
                                                                                                                                  
    data have;                                                                                                                    
     informat drug $9. sex $2. dose;                                                                                              
     input Drug  sex dose;                                                                                                        
    cards4;                                                                                                                       
      Apc      F 152                                                                                                              
      Apc      F 213                                                                                                              
      Apc      F 324                                                                                                              
      Apc      M 535                                                                                                              
      Benedril M 111                                                                                                              
      Benedril F 222                                                                                                              
      Benedril F 334                                                                                                              
      Cafeine  M 145                                                                                                              
      Cafeine  M 251                                                                                                              
      Cafeine  F 312                                                                                                              
      Cafeine  F 425                                                                                                              
      Cafeine  M 542                                                                                                              
      Detene   F 153                                                                                                              
      Detene   M 214                                                                                                              
      Detene   F 425                                                                                                              
      Detene   M 551                                                                                                              
      Efane    F 162                                                                                                              
      Efane    M 274                                                                                                              
      Efane    F 535                                                                                                              
    ;;;;                                                                                                                          
    run;quit;                                                                                                                     
                                                                                                                                  
     WORK.HAVE total obs=19                                                                                                       
                                                                                                                                  
       DRUG        SEX    DOSE                                                                                                    
                                                                                                                                  
       Apc          F      152                                                                                                    
       Apc          F      213                                                                                                    
       Apc          F      324                                                                                                    
       Apc          M      535                                                                                                    
       Benedril     M      111                                                                                                    
       Benedril     F      222                                                                                                    
     ...                                                                                                                          
                                                                                                                                  
    *            _               _                                                                                                
      ___  _   _| |_ _ __  _   _| |_                                                                                              
     / _ \| | | | __| '_ \| | | | __|                                                                                             
    | (_) | |_| | |_| |_) | |_| | |_                                                                                              
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                             
                    |_|                                                                                                           
    ;                                                                                                                             
                                                                                                                                  
                                                                                                                                  
            Apc    Benedril    Cafeine     Detene    Efane                                                                        
    SEX   N = 4       N = 3      N = 5      N = 4    N = 3                                                                        
                                                                                                                                  
    F       689         556        737        578      697                                                                        
    M       535         111        938        765      274                                                                        
                                                                                                                                  
    Sum    1224         667       1675       1343      971                                                                        
                                                                                                                                  
    *                                                                                                                             
     _ __  _ __ ___   ___ ___  ___ ___                                                                                            
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                           
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                           
    | .__/|_|  \___/ \___\___||___/___/                                                                                           
    |_|                                                                                                                           
    ;                                                                                                                             
    proc datasets lib=work nolist;                                                                                                
      delete cor;                                                                                                                 
    run;quit;                                                                                                                     
                                                                                                                                  
    %symdel apc benedril cafeine detene efane;                                                                                    
                                                                                                                                  
    proc sql;                                                                                                                     
         select                                                                                                                   
            resolve(catx(' '                                                                                                      
              ,'%let',drug                                                                                                        
              ,'=%str(',drug,'/N ='                                                                                               
              ,put(count(*),2. -l),');'                                                                                           
          ))                                                                                                                      
          from                                                                                                                    
               have                                                                                                               
         group                                                                                                                    
               by drug;                                                                                                           
    ;quit;                                                                                                                        
                                                                                                                                  
                                                                                                                                  
    ods output observed=cor(rename=(label=sex sum=total));                                                                        
    proc corresp data=have dim=1 observed;                                                                                        
    table sex ,drug;                                                                                                              
    weight dose;                                                                                                                  
    run;quit;                                                                                                                     
                                                                                                                                  
                                                                                                                                  
    proc report data=cor missing nowd split='/';                                                                                  
    cols sex APC BENEDRIL CAFEINE DETENE EFANE;                                                                                   
    define APC      / display  "&APC"      width=6;                                                                               
    define BENEDRIL / display  "&BENEDRIL" width=10;                                                                              
    define CAFEINE  / display  "&CAFEINE"  width=9;                                                                               
    define DETENE   / display  "&DETENE"   width=9;                                                                               
    define EFANE    / display  "&EFANE"    width=7;                                                                               
    ;;;;                                                                                                                          
    run;quit;                                                                                                                     
                                                                                                                                  
                                                                                                                                  
