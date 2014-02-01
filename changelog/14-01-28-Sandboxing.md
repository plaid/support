#January 28, 2014 - Sandboxing, New Banks & Domains
Plaid API, release v2.02

##Overview

We've made some major changes to the API, some of which are breaking to current users. Our previous domain (api-dev.plaid.io) will remain active with the old version for the forseeable future while users migrate.

Changes are as follows:
 * Domain and Route Changes
 * Sandbox Mode
 * Improved Categorization
 * Error Codes
 * MFA Response Structure
 * New Banks

Full documentation can be found [here](https://www.plaid.com/docs).

---

##Domain and Route Changes

###Domains
Going forward, all traffic should be directed to:

```
https://api.plaid.com/      (production)
https://tartan.plaid.com/   (development)
```

###Routes
Routes have been condensed, and only the following are now active:

####Connect
#####Connect Submit:
```POST```  [/connect](https://tartan.plaid.com/connect)  

#####Connect Step:
```POST```  [/connect/step](https://tartan.plaid.com/connect/step)  

#####Connect Get:
```GET```  [/connect](https://tartan.plaid.com/connect/get)  
```POST```  [/connect](https://tartan.plaid.com/connect)  

#####Connect Delete:
```DELETE``` [/connect](https://tartan.plaid.com/connect)


####Auth (formerly Info)
#####Auth Submit:
```POST```  [/auth](https://tartan.plaid.com/auth)

#####Auth Step:
```POST```  [/auth/step](https://tartan.plaid.com/auth/step)

#####Auth Get:
```GET```  [/auth](https://tartan.plaid.com/auth/get)
```POST```  [/auth](https://tartan.plaid.com/auth)

#####Auth Delete:
```DELETE``` [/auth](https://tartan.plaid.com/auth)


####Categories
#####All Categories:
```GET```  [/category](https://tartan.plaid.com/category) (new route)

#####Category by ID
```GET```  [/category/id](https://tartan.plaid.com/category/id/)  

#####Category by Mapping
```GET```  [/category/map](https://tartan.plaid.com/category/map)


---

##Sandbox Mode

A development mode is now available that works with both test user credetials and live accounts. All responses using the plaid_test credentials except for those listed will return as incorrect. Test credentials are full-featured, and should work as normal with the rest of the API, including the return of valid [response and error codes](https://github.com/plaid/support/blob/master/errors.md).

###Credentials:
```
_username_ : ```plaid_test```

_correct password_ : ```plaid_good```  
_locked password_ : ```plaid_locked```  
_bad password_ : _anything else_  
```

####For Question MFA:
```
_correct answer_ : ```tomato```  
_bad answer_ : _anything else_
```

####For Code MFA:
```
_correct answer_ : ```1234```  
_bad answer_ : _anything else_
```

####Other:
For ```/step``` and ```GET``` calls     
*access_token* : ```test```

Additionally, the list and login parameters are fully functional including webhook support.

---

##Categorization

A number of important updates were made to our category endpoint, resulting in higher quality categorization across the board. There is also an 'All Categories' endpoint now available that returns a JSON response containing all Plaid categories and their associated Factual and Foursquare mappings.

```
curl -X GET https://tartan.plaid.com/category
```

A full list of Plaid categories can be found [here](https://github.com/plaid/support/blob/master/categories.md)

---

##Error Codes

We now use standard HTTP response codes for success and failure notifications, and our errors are further classified by block. Errors are returned with a proper numeric code, http code, and description.

```
http code 402
{
  code: 1200,
  message: "invalid credentials",
  resolve: "The username or password provided were not correct."
}
```

A detailed list of error codes and their descriptions can be found [here](https://github.com/plaid/support/blob/master/errors.md).

---

##MFA Responses

When an MFA credential is required, a ```201``` HTTP code will be returned.

###List type
(Applicable on Chase and Bank of America)
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

###Question type
(Applicable on Bank of America)
```
{
  type: "question",
  mfa: { //We should think about moving this to an array
    question: 'What is your mothers maiden name?'
  }
  access_token: "xxxxx"
}
```

###Code type
(Applicable on Chase and Bank of America)
```
{
  type: "code",
  mfa: "Code sent to ' + 'xxx-xxx-9135"
  access_token: "xxxxx"
}
```

---

##Bank Integrations

New banks!

 - Citi:         (type: citi)
 - Wells Fargo:  (type: wells)