# Cost Adjustment

## Requirements 
> ### 1) Which Security Costing (Optional) and How much to Adjust
> ### 2) if Adjustment is more than `1000$` ask back up(Based on Step 2 condition)
> ### 3) Which Month
> ### 4) Accout_no/LOT if to deal with BGMM (step 17)
> ### 5) Trade Date to Deal with BPMM (Step 18)

<br>

## Algorithm
> ### Step 0: Don't work for Cost at timing `2PM to 6:30PM ET` -> `12:30AM to 5:00AM IST`  

> ### Step 1: Go to `BCMC_RECORD` press `d` next to correct fund

> ### Step 2: Inside `BCMC_SCREEN` press `enter` to go to next page where you can find `SECURITY COSTING` 
>> ```javascript
>>  if(BCMC_SCREEN.SECURITY_COSTING != user.SECURITY_COSTING){
>>      Go to Step 12   
>>  }
>>  else(User not Provided Security Costing){
>>      Note down BCMC_SCREEN.SECURITY_COSTING
>>      Go to Step 3   
>>  }
>> ```
>> ### If user not provided Security_Costing note down `BCMC_SCREEN` SECURITY_COSTING

> ### Step 3: Go to `BPOP_RECORD` press `d` next to correct FUND

> ### Step 4: Inside `BPOP_SCREEN` check `LONG or  SHORT POSITION` field and note it down

> ### Step 5: Go to `BRFD_RECORD` press `d` next to correct FUND 

> ### Step 6: Inside `BRFD_SCREEN` note `LONG POSITION` fild

> ### Step 7: If it is for `PREVIOUS MONTH` then you need to check `BRMP_RECORD` also in that go to last page and note down `previous month` one and press `d` next to it

> ### Step 8: Inside `BRMP_SCREEN` note down `LONG POSITION`

> ### Step 9: Go to `BGMM_RECORD` check `STA` 
>> ```javascript
>> if(BGMM_RECORD.STA == C || BGMM_SCREEN.PAR_VALUE == 0){
>>        Note down STA or if PAR
>>  }
>> ```
>> ### Note down BGMM_SCREEN.STA or if 1 note PAR_VALUE 

> ### Step 10: Go to `BPMM_RECORD` and press `d` next to correct Fund. Now you are inside `BPMM_SCREEN` note down `SHR/QTY` field

> ### Step 11: Check all noted value in Step 10,9,8,6 and 4 and Compare with logic
>> ```javascript
>>   if(position of BPOP,BRFD,`BRMP if Previous` == 0 && (BGMM.STA == C or par is 0 ||BPMM_SCREEN.SHR == 0)){
>>       if cost adjustment is more than 1000$ ask for back up
>>       if Satisfied this all Now you can modify, go to Step 12  
>>    }
>>    else{
>>          Request should closed with comment i.e at SOP page 14          
>>      }
>> ```

> ### Step 12: Here you have to be clear about which costing should be edited ie `A or I` if user provided costing method go for it otherwise edit accoring to BCMC_SCREEN SECURITY_COSTING

> ### Step 13: Modify BPOP by `MPOP` clear cost, according to Step 12 that may be id or average by `space bar, input adjusted value then hit Enter` here you need to check two screen within MPOP `so again go to MPOP screen and Press Y in UPDATE LOCAL COST?:` and Do same as above

> ### Step 14: if MPOP is modified it will automatically reflect on BRFD also

> ### Step 15: Modify BRFD by `MRFD` clear cost as of above step

> ### Step 16: If User asked for Previous Month to be Cleared Modify BRMP by `MRMP` in `MPMP_RECORD go to last` press `d` next to it and clear according to step 14   

> ### Step 17: Modify BGMM by `UGMM` at `UGMM_RECORD` press `d` next to correct fund and ACCT/LOT. And Tab down to `ORIGINAL_COST:` field and press `space bar to remove it and input new value`

> ### Step 18: Type BPMM and Input `fund, cusip, Trade_date, LOT` hit enter. Now tab down to COST fields and Edit them.

> ### Step 19: At last all modification close the ticket by resonse in SOP page 24
