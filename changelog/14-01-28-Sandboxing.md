#January 28, 2014 - Sandboxing

###New API - v2.02

New API will include a number of enhancements over the previous version including new bank integrations, enhanced categorization, and separate production and development environments/endpoints.

>```json
> https://api.plaid.com/ 		(production)
> https://tartan.plaid.com/		(development)
>```

Routes are condensing to the following

***Connect***

**Connect Submit**  
```POST```  [/connect](https://tartan.plaid.com/v2/connect)  

**Connect Step**  
```POST```  [/connect/step](https://tartan.plaid.com/v2/connect/step)  

**Connect Get**  
```GET```  [/connect](https://tartan.plaid.com/v2/connect/get)  
```POST```  [/connect](https://tartan.plaid.com/v2/connect)  

**Connect Delete**  
```DELETE``` [/connect](https://tartan.plaid.com/v2/connect)


**Info is rebranding to Auth**  
**Auth Submit**  
```POST```  [/auth](https://tartan.plaid.com/v2/auth)  

**Auth Step**  
```POST```  [/auth/step](https://tartan.plaid.com/v2/auth/step)  

**Auth Get**  
```GET```  [/auth](https://tartan.plaid.com/v2/auth/get)  
```POST```  [/auth](https://tartan.plaid.com/v2/auth)  

**Auth Delete**  
```DELETE``` [/auth](https://tartan.plaid.com/v2/auth)


***Categories***

**All Categories**  
```GET```  [/category](https://tartan.plaid.com/v2/category)  

**Category by ID**  
```GET```  [/category/id](https://tartan.plaid.com/v2/category/id/)  

**Category by Mapping**  
```GET```  [/category/map](https://tartan.plaid.com/v2/category/map)

Full documentation can be found [here](https://www.plaid.com/docs).


###Sandbox Mode

A development mode is now available that works with both test user credetials and live accounts. All responses using the plaid_test credentials except for those listed will return as incorrect. Test credentials are full-featured, and should work as normal with the rest of the API, including the return of valid [response and error codes](https://github.com/plaid/support/blob/master/errors.md).

Credentials:
```
_username_ : ```plaid_test```

_correct password_ : ```plaid_good```  
_locked password_ : ```plaid_locked```  
_bad password_ : _anything else_  
```

For Question MFA:
```
_correct answer_ : ```tomato```  
_bad answer_ : _anything else_
```

For Code MFA:
```
_correct answer_ : ```1234```  
_bad answer_ : _anything else_
```

Other:
For ```/step``` and ```GET``` calls     
*access_token* : ```test```

Additionally, the list and login parameters are fully functional including webhook support.


###Categorization

A number of important updates were made to our category endpoint, resulting in higher quality categorization across the board. There is also an 'All Categories' endpoint now available that returns a JSON response containing all Plaid categories and their associated Factual and Foursquare mappings.

>```json
> curl -X GET https://tartan.plaid.com/v2/category
>```

A full list of Plaid categories can be found [here](https://github.com/plaid/support/blob/master/categories.md)


###Error Codes

We now use standard HTTP response codes for success and failure notifications, and our errors are further classified by block. Errors are now returned with a proper numeric code, http code, and description.

Format will be 

```
http code 402
{
  code: 1200,
  message: "invalid credentials",
  resolve: "The username or password provided were not correct."
}
```

A detailed list of error codes and their descriptions can be found [here](https://github.com/plaid/support/blob/master/errors.md).


###MFA Responses

If an MFA response is need a ```201``` HTTP code will be returned.
New format is as follows.

*List type* (Applicable on Chase and Bank of America)
```
{
  type: "list",
  mfa: [
    {mask:"w...m@plaid.com", type:"email"},
    {mask:"xxx-xxx-9135", type:"phone"},
    {mask:"SafePass Card", type:"card"}
  ]
  access_token: "xxxxx"
}
```

*Question type* (Applicable on Bank of America)
```
{
  type: "question",
  mfa: { //We should think about moving this to an array
    question: 'What is your mothers maiden name?'
  }
  access_token: "xxxxx"
}
```

*Code type* (Applicable on Chase and Bank of America)
```
{
  type: "code",
  mfa: "Code sent to ' + 'xxx-xxx-9135"
  access_token: "xxxxx"
}
```

###Bank Integrations

Our current bank integrations now include:

 - American Express		(type: amex)
 - Bank of America 		(type: bofa)
 - Chase				(type: chase)
 - Citi:				(type: citi)
 - Wells Fargo			(type: wells)

Capital One will be available soon, with more banks on the way.