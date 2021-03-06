# [Hot Recharge](https://ssl.hot.co.zw/)
library to do hot recharge transactions via a web service rest api

- ℹ Not an official hot-recharge python library

## Library installation
```sh
$ pip install -U hot-recharge
```

### Sign Up
- needs a hot recharge co-operate account, sign up [here](https://ssl.hot.co.zw/register.aspx)
![sign up](Docs/images/signup_cooperate.png)

### doing get requests
- this shows how to do basic get requests for different services
```python
import hotrecharge
import pprint

# pass dict keys as is, change dict values.. <fix coming soon>
credentials = {
    'code': '<your-code>',
    'pswd': '<your-password',
    'ref': '<agent-reference>'
}

# to use random code generated references, flag it to True, reccommended
api = hotrecharge.HotRecharge(headers=credentials, use_random_ref=True)

try:
    # get wallet balance
    wallet_bal_response = api.walletBalance()

    # get end user balance
    end_user_bal_resp = api.endUserBalance(mobile_number='077xxxxxxx')

    # get data bundles
    data_bundles_resp = api.getDataBundles()

    print("Wallet Balance: ")
    pprint.pprint(wallet_bal_response)

    print("End User Balance: ")
    pprint.pprint(end_user_bal_resp)

    print("Data Bundles Balance: ")
    pprint.pprint(data_bundles_resp)

except Exception as ex:
    print(f"There was a problem: {ex}")
```
-
# Recharge
## Recharge data bundles
- use bundle product code
- an optional customer sms can be send together upon request
- Place holders used include
```
%AMOUNT% 	$XXX.XX
%COMPANYNAME%	As Defined by Customer on the website www.hot.co.zw
%ACCESSNAME%	Defined by Customer on website – Teller or Trusted User or branch name
%BUNDLE%	Name of the Data Bundle
```
```python
import hotrecharge
import sys
from pprint import pprint

credentials = {
    'code': '<your-code>',
    'pswd': '<your-password',
    'ref': '<agent-reference>'
}

# to use random code generated references, flag it to True
api = hotrecharge.HotRecharge(headers=credentials, use_random_ref=False)

try:
    # recharge data bundle with a (OPTIONAL) custom Customer SMS, max char should not exceed 135
    # uses places holders as used on hot recharge registration
    customer_sms =  " Amount of %AMOUNT% for data %BUNDLE% recharged! " \
                    " %ACCESSNAME%. The best %COMPANYNAME%!"

    # can check length first if not sure
    if len(customer_sms) > 135:
        print("Too many chars for custom customer sms.")
        sys.exit()

    # need to update reference manually, if `use_random_ref` is set to False
    api.updateReference('383yd3')

    response = api.dataBundleRecharge(product_code="<bundle-product-code>", number="077xxxxxxx", mesg=customer_sms)

    pprint(response)

except Exception as ex:
    print(f"There was a problem: {ex}")
```

### recharge pinless
```python
import hotrecharge
import sys

credentials = {
    'code': '<your-code>',
    'pswd': '<your-password',
    'ref': '<agent-reference>'
}

# to use random code generated references, flag it to True
api = hotrecharge.HotRecharge(headers=credentials, use_random_ref=False)

try:
    # recharge pinless with a (OPTIONAL) custom Customer SMS, max char should not exceed 135
    # uses places holders as used on hot recharge registration
    customer_sms = "Recharge of %AMOUNT% successful" \
                   "Initial balance $%INITIALBALANCE%" \
                   "Final Balance $%FINALBALANCE%" \
                   "Thank you for using %COMPANYNAME%"

    # can check length first if not sure
    if len(customer_sms) > 135:
        print("Too many chars for custom customer sms.")
        sys.exit()

    # can update reference manually, if `use_random_ref` is set to False
    api.updateReference('37ehd93')

    response = api.rechargePinless(amount=0.5, number="077xxxxxxx", mesg=customer_sms)

    print(response)

    pass

except Exception as ex:
    print(f"There was a problem: {ex}")
```
