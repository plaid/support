# Link and API errors

## Table of Contents

- [Link and API errors](#link-and-api-errors)
- [Invalid request errors](#invalid-request-errors)
- [Invalid input errors](#invalid-input-errors)
- [Rate limit exceeded errors](#rate-limit-exceeded-errors)
- [API errors](#api-errors)
- [Item errors](#item-errors)
- [Legacy API errors](legacy-errors.md)

All errors returned by Link and the API include the following data elements:

|       Field       |                                                                                      Description                                                                                       |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `error_type`      | A broad categorization of the error. One of: `INVALID_REQUEST`, `INVALID_INPUT`, `RATE_LIMIT_EXCEEDED`, `API_ERROR`, or `ITEM_ERROR`. Safe for programmatic use.                       |
| `error_code`      | The particular error code. Each `error_type` has a specific set of `error_code`s. <br /><br />  Safe for programmatic use.                                                             |
| `error_message`   | A developer-friendly representation of the error message.<br /><br />  This may change over time and is not safe for programmatic use.                                                 |
| `display_message` | A user-friendly representation of the error message. `null` if the error is not related to user action. <br /><br />   This may change over time and is not safe for programmatic use. |

**Looking for error codes for our legacy API?** See the [legacy errors][legacy-errors].

## Invalid request errors

Returned when the request is malformed and cannot be processed.

**Error type:** `INVALID_REQUEST`

|     Error code    | Status code |                                                               Notes                                                               |
|-------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `MISSING_FIELDS`  |         400 | The request was missing one or more required fields. The `error_message` field will list the missing field(s).                    |
| `UNKNOWN_FIELDS`  |         400 | The request included a field that is not recognized by the endpoint. The `error_message` field will list the extraneous field(s). |
| `INVALID_FIELD`   |         400 | One or more of the request body fields was improperly formatted or an invalid type.                                               |
| `INVALID_BODY`    |         400 | The request body was invalid. Only JSON bodies are accepted.                                                                      |
| `INVALID_HEADERS` |         400 | The request was missing a required header, most typically the `content-type` header.                                              |
| `NOT_FOUND`       |         404 | The endpoint requested does not exist.                                                                                            |
| `SANDBOX_ONLY`    |         400 | The requested endpoint is only available in the [Sandbox API environment](https://plaid.com/docs/api#sandbox).                    |

## Invalid input errors

**Error type:** `INVALID_INPUT`

Returned when all fields are provided and are in the correct format, but the values provided are incorrect in some way.

|       Error code       | Status code |                                                                                                          Notes                                                                                                           |
|------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `INVALID_API_KEYS`     |         400 | The client ID and secret included in the request body were invalid.                                                                                                                                                      |
| `INVALID_ACCESS_TOKEN` |         400 | Access tokens are in the format:<br /><br />`access-<environment>-<identifier>`<br /><br />This error can happen when the `access_token` you provided is invalid, from a different API environment, or has been deleted. |
| `INVALID_PUBLIC_TOKEN` |         400 | Public tokens are in the format:<br /><br />`public-<environment>-<identifier>`<br /><br />This error can happen when the `public_token` you provided is invalid, from a different API environment, or expired.          |
| `UNAUTHORIZED_PRODUCT` |         400 | Your client ID does not have access to this product. <a href="https://plaid.com/contact/sales">Contact us</a> to gain access.                                                                                            |
| `INVALID_ACCOUNT_ID`   |         400 | One of the `account_id`(s) specified is invalid or does not exist.                                                                                                                                                       |
| `INVALID_INSTITUTION`  |         400 | The `institution_id` specified is invalid or does not exist.                                                                                                                                                             |

## Rate limit exceeded errors

Returned when the request is valid but has exceeded established rate limits. All API endpoints are rate limited and all data-access endpoints are rate limited by `access_token`. Exact limits are dynamic and are designed to prevent any single source of traffic from impacting overall API stability.

**Error type:** `RATE_LIMIT_EXCEEDED`

|      Error code      | Status code |                                                                                Notes                                                                                |
|----------------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ADDITION_LIMIT`     |         429 | You have exceeded your addition limit in our Development environment. To increase it, or raise it from zero, [contact us](https://dashboard.plaid.com/support/new). |
| `AUTH_LIMIT`         |         429 | Too many requests were made to the `/auth/get` endpoint.                                                                                                            |
| `TRANSACTIONS_LIMIT` |         429 | Too many requests were made to the `/transactions/get` endpoint.                                                                                                    |
| `IDENTITY_LIMIT`     |         429 | Too many requests were made to the `/identity/get` endpoint.                                                                                                        |
| `INCOME_LIMIT`       |         429 | Too many requests were made to the `/income/get` endpoint.                                                                                                          |
| `RATE_LIMIT`         |         429 | Too many requests were made.                                                                                                                                        |
| `ITEM_GET_LIMIT`     |         429 | Too many requests were made to the `/item/get` endpoint.                                                                                                            |


## API errors

Returned during planned maintenance windows and in response to API errors.

**Error type:** `API_ERROR`

|        Error code       | Status code |                                                                       Notes                                                                        |
|-------------------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| `INTERNAL_SERVER_ERROR` |         500 | Plaid was unable to process the request, either due to an internal system issue or an unsupported response from a financial institution.           |
| `PLANNED_MAINTENANCE`   |         500 | Plaid's systems are in maintenance mode and API operations are disabled. Advanced notice will be provided if a maintenance window is ever planned.x |

## Item errors

An `ITEM_ERROR` indicates that information provided for the Item (such as credentials or MFA) may be invalid or that the Item is not supported on Plaid's platform. You'll encounter `ITEM_ERROR`s as your users use Link via the [`onExit()` callback](https://plaid.com/docs/api#onexit-callback).

**Error type:** `ITEM_ERROR`

|             Error code            | Status code |                                                                                                             Notes                                                                                                              |
|-----------------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `INVALID_CREDENTIALS`             |         400 | The financial institution indicated that the credentials provided were invalid.                                                                                                                                                |
| `INVALID_MFA`                     |         400 | The financial institution indicated that the MFA response(s) provided were invalid.                                                                                                                                            |
| `INSTITUTION_DOWN`                |         400 | The financial institution is down, either for maintenance or due to an infrastructure issue with their systems.                                                                                                                |
| `INSTITUTION_NOT_RESPONDING`      |         400 | The financial institution is failing to respond to some of our requests. Your user may be successful if they retry another time.                                                                                               |
| `INSTITUTION_NOT_AVAILABLE`       |         400 | The financial institution is not available this time.                                                                                                                                                                          |
| `INSTITUTION_NO_LONGER_SUPPORTED` |         400 | Plaid no longer supports this financial institution.                                                                                                                                                                           |
| `ITEM_LOCKED`                     |         400 | The financial institution indicated that the user's account is locked. The user will need to work with the institution directly to unlock their account.                                                                       |
| `ITEM_LOGIN_REQUIRED`             |         400 | The financial institution indicated that the user's password or MFA information has changed. They will need to reauthenticate via Link's [update mode](https://plaid.com/docs/api#updating-items-via-link)                     |
| `ITEM_NOT_SUPPORTED`              |         400 | Plaid is unable to support this user's accounts due to restrictions in place at the financial institution.                                                                                                                     |
| `USER_SETUP_REQUIRED`             |         400 | The user must log in directly to the financial institution and take some action (agree to updated terms and conditions, change their password, etc.) before Plaid can access their accounts.                                   |
| `MFA_NOT_SUPPORTED`               |         400 | Returned when the user requires a form of MFA that Plaid does not support for a given financial institution.                                                                                                                   |
| `NO_ACCOUNTS`                     |         400 | Returned when the there are no open accounts associated with the `Item`.                                                                                                                                                       |
| `PRODUCT_NOT_READY`               |         400 | Returned when a data request has been made for a product that is not yet ready. This primarily happens when attempting to pull transactions prior to the [initial pull](https://plaid.com/docs/api#item-transaction-lifecyle). |

[errors]: https://plaid.com/docs/api#errors
[legacy-errors]: legacy-errors.md
