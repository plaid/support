#January 28, 2014 - Sandboxing

###New API - v2.02

New API will include a number of enhancements over the previous version including new bank integrations, enhanced categorization, and separate production and development environments/endpoints.

>```json
> https://api.plaid.com/ 		(production)
> https://tartan.plaid.com/	(development)
>```

Full documentation can be found [here](https://www.plaid.com/docs).


###Sandbox Mode

A development mode is now available that works with both test user credetials and live accounts. All responses using the plaid_test credentials except for those listed will return as incorrect. Test credentials are full-featured, and should work as normal with the rest of the API, including the return of valid [response and error codes](https://github.com/plaid/support/blob/master/errors.md).

Credentials:
 - username			plaid_test
 - password			plaid_good (correct), plaid_locked (locked)

 MFA:
  - access_token	test
  - question-based	tomato
  - code-based		1234

Additionally, the list and login parameters are fully functional including webhook support.


###Categorization

A number of important updates were made to our category endpoint, resulting in higher quality categorization across the board. There is also an 'All Categories' endpoint now available that returns a JSON response containing all Plaid categories and their associated Factual and Foursquare mappings.

>```json
> curl -X GET https://tartan.plaid.com/v2/category
>```

A full list of Plaid categories can be found [here](https://github.com/plaid/support/blob/master/categories.md)


###Error Codes

We now use standard HTTP response codes for success and failure notifications, and our errors are further classified by block. Errors are now returned with a proper numeric code, http code, and description.

A detailed list of error codes and their descriptions can be found [here](https://github.com/plaid/support/blob/master/errors.md).


###Bank Integrations

Our current bank integrations now include:

 - American Express		(type: amex)
 - Bank of America 		(type: bofa)
 - Chase				(type: chase)
 - Citi:				(type: citi)
 - Wells Fargo			(type: wells)

Capital One will be available soon, with more banks on the way.