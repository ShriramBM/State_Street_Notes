# Invalid Line Removal

## Requirements
> ### 1. A copy of the `Interest Receivable Report`, Highlighting which interest lines are invalid.
> ### 2. Which Month

<br>

## ALGORITHM

> ### Step 1: If the Request is for funds with client ID `EMXX`, `SVXX` or `24ZZ` - STOP

> ### Step 2: Go to `BGMM_RECORD` note `STA` 
>> ```javascript
>> if(STA == C || (STA == 1 && BGMM_SCREEN.PAR and COST values != 0)){
>>          Go To Step 3
>>    }
>> else if(STA == 1){
>>      Go to Step 4
>>  }
>>  else if(No record found)
>>      Go to Step 
>> ```

> ### Step 3: If STA is C  or `If STA is 1 but there is PAR and COST in BGMM_SCREEN` Modify the `ACCRUAL TOTAL` to `0`
>> ```javascript
>> if(Given_month == Previous){
>>      Modify 
>>         BRFA -> MRFA
>>         BRMA -> MRMA             
>>  }
>> else if(Given_month == Current){
>>      Modify
>>          BRFA -> MRFA    
>>}
>>```

> ### Step 4: If STA is 1 Go inside `BGMM_SCEEN` and go to `next Page` Note down `ACCRUAL` , `ACCOUNT` and `ACCRUAL TOTAL`
>>```javascript
>> if(ACCRUAL == N){
>>      Go to Step 5  
>>} 
>> else if(ACCRUAL == Y){
>>      Go to Step 9
>>}
>>```

> ### Step 5: If Accrual is N Go to `BRKP_RECORD` Record and Check `BUY` TYPE with same `ACCOUNT NO.` Check all `pages` by `PF5`  

> ### Step 6: Inside `BRKP_SCREEN` note `INTEREST:` field And `Press PF7` key to Identify Sell 

> ### Step 7: Press `d` to detail it, Here you need to Check 
>>```javascript
>> var sum=0;
>> if(SELLS_RECORD is More){
>>    if(CXLED == Y)
>>          Ignore it
>>     else
>>          sum += BRKP_SCREEN.SELL.INTEREST
>>  }
>>  else if(SELLS_RECORD == 1) {
>>      if(CXLED == Y)
>>          Ignore it
>>      else 
>>          Note down BRKP_SCREEN.SELL.INTEREST      
>>  }
>>```
>> ### Note down the `SUM` or single valued sell `INTEREST`

> ### Step 8: Now Compare the `ACCRUAL TOTAL` at step 4 and `SUM or Single valued Sell` at step 7 minus `INTEREST` at 6
>>``` javascript
>>  if((BRKP.INTEREST - BRKP.SELL_SUM) == BGMM.ACCRUAL){
>>      Request should be Close with comment in SOP i.e page no 14     
>>  }
>>  else {
>>      Go to Step 3
>>  }
>>```

> ### Step 9: If Accrual is Y Go to `BRKP` Record and Check `BUY` TYPE with same `ACCOUNT NO.`. Check all `pages` by `PF5` and Press `d` to Detail it

> ### Step 10: Inside `BRKP_SCREEN` note down `ACT SET DT` and Press `PF7` to see Sells

> ### Step 11: Press `d` and next to it sells and check the `CXLED` field and Note all `LOT` which are not canceled
>>```javascript
>>  var lot = [];
>>  var j=0;
>>  for(let i=0;i<BRKP.SELL_RECORD;i++){
>>  if(BRKP.SELL.CXLED == N){
>>      lot[j] = BRKP.SELL.LOT[j];
>>      j++;              
>>  }
>>}
>>``` 

> ### Step 12: Press `PF11` key Three time to go back for `BRKP_RECORD` now Check the `BUY` type with noted `LOT` at step 11. Here you need to Check only `BUY` type with `ACCT` which is Previously noted

> ### Step 13: Press `d` next that satisfied `above` step ie 12, And Check `ACT SET DT` field
>> ```javascript
>>  if(BRKP_RECORD.BUY.ACT_SET_DT == 000000){
>>      Request should be closed with following comment i.e SOP page 21  
>>  }
>>  else{
>>      Go to Step 3        
>>  }
>>```  