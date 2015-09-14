# Error Codes

Below you will find details for our various response codes.

HTTP | Code | Message | Resolve
---- | ---- | ------- | -------
`400` | `1000` | *access_token missing* | You need to include the access_token that you received from the original submit call.
`400` | `1001` | *type missing* | You need to include a type parameter. Ex. bofa, wells, amex, chase, citi, etc.
`400` | `1003` | *access_token disallowed* | You included an access_token on a submit call - this is only allowed on step and get routes.
`400` | `1008` | *unsupported access_token* | This access token format is no longer supported. Contact support to resolve.
`400` | `1004` | *invalid options format* | Options need to be JSON or stringified JSON.
`400` | `1005` | *credentials missing* | Provide username, password, and pin if appropriate.
`400` | `1006` | *invalid credentials format* | Credentials need to be JSON or stringified JSON.
`400` | `1007` | *upgrade_to required* | In order to upgrade an account, an upgrade_to field is required , ex. connect
`400` | `1009` | *invalid content-type* | Valid 'Content-Type' headers are 'application/json' and 'application/x-www-form-urlencoded' with an optional 'UTF-8' charset.
`401` | `1100` | *client_id missing* | Include your Client ID so we know who you are.
`401` | `1101` | *secret missing* | Include your Secret so we can verify your identity.
`401` | `1102` | *secret or client_id invalid* | The Client ID does not exist or the Secret does not match the Client ID you provided.
`401` | `1104` | *unauthorized product* | Your Client ID does not have access to this product. Contact us to purchase this product.
`401` | `1105` | *bad access_token* | This access_token appears to be corrupted.
`401` | `1106` | *bad public_token* | This public_token is corrupt or does not exist in our database. See https://github.com/plaid/link for docs.
`401` | `1107` | *missing public_token* | Include the public_token received from the Plaid Link module. See https://github.com/plaid/link for docs.
`401` | `1108` | *invalid type* | This institution is not currently supported.
`401` | `1109` | *unauthorized product* | The sandbox client_id and secret can only be used with sandbox credentials and access tokens. See https://plaid.com/docs/#sandbox.
`401` | `1110` | *product not enabled* | This product is not enabled for this item. Use the upgrade route to add it.
`401` | `1111` | *invalid upgrade* | Specify a valid product to upgrade this item to.
`401` | `1112` | *addition limit exceeded* | You have reached the maximum number of additions. Contact us to raise your limit.
`429` | `1113` | *rate limit exceeded* | You have exceeded your request rate limit for this product. Try again soon.
`402` | `1200` | *invalid credentials* | The username or password provided were not correct.
`402` | `1201` | *invalid username* | The username provided was not correct.
`402` | `1202` | *invalid password* | The password provided was not correct.
`402` | `1203` | *invalid mfa* | The MFA response provided was not correct.
`402` | `1204` | *invalid send_method* | The MFA send_method provided was invalid. Consult the documentation for the proper format.
`402` | `1205` | *account locked* | The account is locked. Prompt the user to visit the issuing institution's site and unlock their account.
`402` | `1206` | *account not setup* | The account has not been fully set up. Prompt the user to visit the issuing institution's site and finish the setup process.
`402` | `1207` | *country not supported* | We're United States-only at this point!
`402` | `1208` | *mfa not supported* | This account requires MFA to access - we're currently not supporting MFA through this institution.
`402` | `1209` | *invalid pin* | The pin provided was not correct.
`402` | `1210` | *account not supported* | This account is currently not supported.
`402` | `1211` | *bofa account not supported* | The security rules for this account restrict access. Disable 'Extra Security at Sign-In' in your Bank of America settings.
`402` | `1212` | *no accounts* | No valid accounts exist for this user.
`402` | `1214` | *invalid state* | We either could not understand the state you sent or the state is not supported by the institution.
`402` | `1215` | *mfa reset* | MFA access has changed or this application's access has been revoked. Submit a PATCH call to resolve.
`401` | `1218` | *mfa not required* | This item does not require the MFA process at this time.
`404` | `1300` | *institution not available* | This institution is not yet available in this environment.
`404` | `1301` | *unable to find institution* | Double-check the provided institution ID.
`402` | `1302` | *institution not responding* | The institution is failing to respond to our request, if you resubmit the query the request may go through.
`402` | `1303` | *institution down* | The institution is down for an indeterminate amount of time, if you resubmit in a couple hours it may go through.
`404` | `1501` | *unable to find category* | Double-check the provided category ID.
`400` | `1502` | *type required* | You must include a type parameter.
`400` | `1503` | *invalid type* | The specified type is not supported.
`400` | `1507` | *invalid date* | Consult the documentation for valid date formats.
`404` | `1600` | *product not found* | This product doesn't exist yet, we're actually not sure how you reached this error...
`404` | `1601` | *product not available* | This product is not yet available for this institution.
`404` | `1605` | *user not found* | User was previously deleted from our system.
`404` | `1606` | *account not found* | The account ID provided was not correct.
`404` | `1610` | *item not found* | No matching items found; go add an account!
`501` | `1700` | *extractor error* | We failed to pull the required information from the institution - make sure the user can access their account; we have been notified.
`502` | `1701` | *extractor error retry* | We failed to pull the required information from the institution - resubmit this query.
`500` | `1702` | *plaid error* | An unexpected error has occurred on our systems; we've been notified and are looking into it!
`503` | `1800` | *planned maintenance* | Portions of our system are down for maintence. This route is inaccessible. GET requests to Auth and Connect may succeed.
