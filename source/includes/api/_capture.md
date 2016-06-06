# Capture Payment
When you charge a customer you have the option of reserving the amount of the purchase without actually deducting that amount from their credit card. In most cases this is the default setting when using the Bambora Native SDKs. When you want to deduct that amount from the customer's card, that is called a **Capture**. You can capture for the full amount or a smaller amount.

There is a time limit on how long you have hold the payment. Eventually the purchase will expire and you won't be able to capture it. This can depend on the card issuer and the acquiring bank. Usually you will have about 1 week to capture the payment. For specifics on this time frame, please [email](mailto:sdk@bambora.com) our support team.

**[Click](https://github.com/bambora/dev.bambora.com/blob/master/source/includes/api/_capture.md) to edit this section.**

## Request

```python
import requests
​
MERCHANT_ACCOUNT = '<MERCHANT_NUMBER>'
MERCHANT_TOKEN = '<MERCHANT_TOKEN>'
MERCHANT_SECRET = '<MERCHANT_SECRET>'
CAPTURE_URL = 'https://api-beta.bambora.com/payments/{payment_reference}/capture/'
​
response = requests.post(
    CAPTURE_URL.format(payment_reference='<PAYMENT_REFERENCE>'),
    auth=('{}@{}'.format(MERCHANT_TOKEN, MERCHANT_NUMBER), MERCHANT_SECRET),
    json={'amount': 2000},
    headers={'API-Version': '1'}
)
```

```shell
PAYMENT_REFERENCE='<PAYMENT_REFERENCE>'
MERCHANT_NUMBER='<MERCHANT_NUMBER>'
MERCHANT_TOKEN='<MERCHANT_TOKEN>'
MERCHANT_SECRET='<MERCHANT_SECRET>'
CAPTURE_URL='https://api-beta.bambora.com/payments/'$PAYMENT_REFERENCE'/capture/'
​
AUTH='Authorization: Basic '$(echo -n $MERCHANT_TOKEN@$MERCHANT_NUMBER:$MERCHANT_SECRET | base64)
​
curl \
    --header "$AUTH" \
    --header 'API-Version: 1' \
    --header 'Content-Type: application/json' \
    --data '{"amount": 2000}' \
    $CAPTURE_URL
```

Our REST API contains a /capture/ endpoint that you can use in order to capture specific payments.

You will need the following data in order to make the request:

  * A merchant number.
  * A merchant token. 
  * A merchant secret.
  * A payment reference (the reference that you set in the SDK before the payment in question was made)

You will get access to the merchant number, a merchant token and a merchant secret after registering with Bambora.

The payment reference refers to the one that you are required set in the SDK before making a payment. In order to capture a payment, you need to provide its unique payment reference.

We have created code examples showing how to capture a payment - one written in python and the other written in bash using CUrl. Please note that each placeholder needs to be replaced with real data.


> The Python code example requires that the [requests library for Python.com](https://github.com/kennethreitz/requests/) is installed on the computer that is running the code.

## Response

If the capture was successful you will receive an HTTP status code of 200 (OK). Any errors or problems will represent themselves as a non-200 status code. Along with the [standard error codes](./api.html#errors), these are the specific responses for `/capture` that you may encounter:

Response Code | Meaning
---------- | -------
200 | OK -- Successful capture, hooray!
402 | Cannot capture -- The capture request could not be performed.
409 | Payment operation blocked -- The payment was being modified by another request. The attempted operation could be retried again, or the payment could be queried to find out if its properties have changed.
422 | Invalid payment state transition -- The state of the payment could not be changed in the way that the payment operation would require.
