# utl-how-to-output-the-standard-error-of-odds-ratio-for-proc-logistic
How to output the standard error of odds ratio for proc logistic
    How to output the standard error of odds ratio for proc logistic                                                                    
                                                                                                                                        
     Solution by                                                                                                                        
        https://stackoverflow.com/users/1249962/richard                                                                                 
                                                                                                                                        
    GitHub                                                                                                                              
    https://cutt.ly/zgGqnNH                                                                                                             
    https://github.com/rogerjdeangelis/utl-how-to-output-the-standard-error-of-odds-ratio-for-proc-logistic                             
                                                                                                                                        
    SAS Forum                                                                                                                           
    https://cutt.ly/BgF6S5M                                                                                                             
    https://stackoverflow.com/questions/64690907/how-to-output-the-standard-error-of-odds-ratio-for-proc-logistic                       
                                                                                                                                        
    *                                                                                                                                   
    #####  #   #  ####   #   #  #####                                                                                                   
      #    ##  #  #   #  #   #    #                                                                                                     
      #    # # #  #   #  #   #    #                                                                                                     
      #    #  ##  ####   #   #    #                                                                                                     
      #    #   #  #      #   #    #                                                                                                     
      #    #   #  #      #   #    #                                                                                                     
    #####  #   #  #       ###     #                                                                                                     
    ;                                                                                                                                   
                                                                                                                                        
    data have;                                                                                                                          
       length Program $ 9;                                                                                                              
       input School Program $ Style $ Count @@;                                                                                         
    cards4;                                                                                                                             
    1 regular   self 10  1 regular   team 17  1 regular   class 26                                                                      
    1 afternoon self  5  1 afternoon team 12  1 afternoon class 50                                                                      
    2 regular   self 21  2 regular   team 17  2 regular   class 26                                                                      
    2 afternoon self 16  2 afternoon team 12  2 afternoon class 36                                                                      
    3 regular   self 15  3 regular   team 15  3 regular   class 16                                                                      
    3 afternoon self 12  3 afternoon team 12  3 afternoon class 20                                                                      
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
    *                                                                                                                                   
     ###   #   #  #####  ####   #   #  #####                                                                                            
    #   #  #   #    #    #   #  #   #    #                                                                                              
    #   #  #   #    #    #   #  #   #    #                                                                                              
    #   #  #   #    #    ####   #   #    #                                                                                              
    #   #  #   #    #    #      #   #    #                                                                                              
    #   #  #   #    #    #      #   #    #                                                                                              
     ###    ###     #    #       ###     #                                                                                              
    ;                                                                                                                                   
                                                                                                                                        
     WORK.WANT total obs=6                                                 ** 95% CIs on Odds ratio **                                  
                                                                                                                                        
                              EFFECT                             ODDSRATIOEST    LOWERCL    UPPERCL                                     
                                                                                                                                        
       STYLE self: PROGRAM regular vs afternoon at SCHOOL=1         3.84615      1.18959    12.4353                                     
       STYLE team: PROGRAM regular vs afternoon at SCHOOL=1         2.72436      1.13242     6.5542                                     
       STYLE self: PROGRAM regular vs afternoon at SCHOOL=2         1.81731      0.79793     4.1390                                     
       STYLE team: PROGRAM regular vs afternoon at SCHOOL=2         1.96154      0.80171     4.7993                                     
       STYLE self: PROGRAM regular vs afternoon at SCHOOL=3         1.56250      0.57241     4.2651                                     
       STYLE team: PROGRAM regular vs afternoon at SCHOOL=3         1.56250      0.57241     4.2651                                     
                                                                                                                                        
    *                                                                                                                                   
    ####   ####    ###    ###   #####   ###    ###                                                                                      
    #   #  #   #  #   #  #   #  #      #   #  #   #                                                                                     
    #   #  #   #  #   #  #      #       #      #                                                                                        
    ####   ####   #   #  #      ####     #      #                                                                                       
    #      # #    #   #  #      #         #      #                                                                                      
    #      #  #   #   #  #   #  #      #   #  #   #                                                                                     
    #      #   #   ###    ###   #####   ###    ###                                                                                      
    ;                                                                                                                                   
                                                                                                                                        
    proc datasets lib=work;                                                                                                             
     delete want;                                                                                                                       
    run;quit;                                                                                                                           
                                                                                                                                        
    ods graphics off;                                                                                                                   
                                                                                                                                        
    proc logistic data=school;                                                                                                          
       freq Count;                                                                                                                      
       class School Program(ref=first);                                                                                                 
       model Style(order=data)=School Program School*Program / link=glogit;                                                             
       oddsratio program / cl=wald;                                                                                                     
                                                                                                                                        
       ods output OddsRatiosWald=want(drop=unit);                                                                                       
    run;                                                                                                                                
                                                                                                                                        
    proc print data=or_program;                                                                                                         
      title1 "Logistic Odds Ratios CL=Wald output data";                                                                                
      title2 "Odds Ratio Estimate 95% Confidence Limits";                                                                               
    run;                                                                                                                                
    title;                                                                                                                              
                                                                                                                                        
                                                                                                                                        
