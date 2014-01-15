
#Payscape Gateway CakePHP Plugin
Rapid eCommerce web development with CakePHP and the Payscape Gateway

## Version
Payscape Direct Post API CakePHP Plugin v3.0
	  
## Set Up	  
* Copy /webroot/crt to your document root.

* Place this Plugin in your app/Plugin directory.

* Edit Config/payscape.php:
*userid* = your Payscape username
*userpass* = your Payscape password

* Move /Config/payscape.php to your /app/Config folder.
	  
* Load the Plugin in your Config/bootstrap file. 
```	  
CakePlugin::load('Payscape');
```	  

* Include the Payscape Component in your Controller 
```
public $components = array('Payscape.Payscape');
```	  

## cURL Notes
* /webroot/crt/cacert.pem is included so that you may use cURL. 
	  
* You may use either cURL or Cake's HTTPSocket for your send() function.
* Both are included here. 
	  
## Features	  
* Sale() detects if your transaction is Credit Card or eCheck and sends the correct params 

* Two send() methods are included, one that uses Cake's HTTPSocket, as well as one that uses cURL.

* To use the Cake HTTPSocket version, simply rename sendHTTPSocket() to send(), and the current send() to sendcURL(). 
	  
* Payscape Gateway CakePHP Plugin exposes all of the methods of the Payscape Gateway
	  
* See Payscape Direct Post API Documentation for complete notes on variables here http://payscape.com/developers/direct-post-api.php

* See the Payscape Gateway CakePHP Developers Suite for examples of each of the methods.

## Documentation

### Example Sale Credit Card transaction
```
$incoming = array();
/* required fields*/
$incoming['amount'] = 'amount';
$incoming['ccexp'] = 'ccexp';
$incoming['ccnumber'] = 'ccnumber';

/* optional fields*/
$incoming['cvv'] = $this->request->data['Transaction']['cvv'];					
$incoming['orderdescription'] = $this->request->data['Transaction']['orderdescription'];
$incoming['orderid'] = $this->request->data['Transaction']['orderid'];

$incoming['firstname'] = $this->request->data['Transaction']['firstname'];
$incoming['lastname'] = $this->request->data['Transaction']['lastname'];
$incoming['company'] = $this->request->data['Transaction']['company'];
$incoming['address1'] = $this->request->data['Transaction']['address1'];
$incoming['city'] = $this->request->data['Transaction']['city'];
$incoming['state'] = $this->request->data['Transaction']['state'];
$incoming['zip'] = $this->request->data['Transaction']['zip'];
$incoming['country'] = $this->request->data['Transaction']['country'];
$incoming['phone'] = $this->request->data['Transaction']['phone'];
$incoming['fax'] = $this->request->data['Transaction']['fax'];
$incoming['email'] = $this->request->data['Transaction']['email'];
	
$result_array = $this->Payscape->Sale($incoming);

```

### Example Response: Sale Credit Card Success
```
 Array
(
    [response] => 1
    [responsetext] => SUCCESS
    [authcode] => 123456
    [transactionid] => 2114572847
    [avsresponse] => 
    [cvvresponse] => 
    [orderid] => 
    [type] => sale
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```	  
### Example Sale eCheck ACH transaction
```
$incoming = array();
$incoming['amount'] = 'amount';
$incoming['type'] = 'sale';
$incoming['payment'] = 'check';
				
$incoming['checkname'] = 'checkname';						
$incoming['checkaba'] = 'checkaba';
$incoming['checkaccount'] = 'checkaccount';
$incoming['account_holder_type'] = 'account_holder_type';
$incoming['account_type'] = 'account_type';
$incoming['sec_code'] = 'WEB';

// optional fields
$incoming['orderid'] = 'orderid';
$incoming['orderdescription'] = 'orderdescription';
			
$incoming['firstname'] = $this->request->data['Transaction']['firstname'];
$incoming['lastname'] = $this->request->data['Transaction']['lastname'];
$incoming['company'] = $this->request->data['Transaction']['company'];
$incoming['address1'] = $this->request->data['Transaction']['address1'];
$incoming['city'] = $this->request->data['Transaction']['city'];
$incoming['state'] = $this->request->data['Transaction']['state'];
$incoming['zip'] = $this->request->data['Transaction']['zip'];
$incoming['country'] = $this->request->data['Transaction']['country'];
$incoming['phone'] = $this->request->data['Transaction']['phone'];
$incoming['fax'] = $this->request->data['Transaction']['fax'];
$incoming['email'] = $this->request->data['Transaction']['email'];
		
$result_array = $this->Payscape->Sale($incoming);
		
```
### Example Response: Sale eCheck ACH Success
```
 Array
(
    [response] => 1
    [responsetext] => SUCCESS
    [authcode] => 123456
    [transactionid] => 2114572847
    [avsresponse] => 
    [cvvresponse] => 
    [orderid] => 
    [type] => sale
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```
### Example Auth transaction
```
$incoming = array();
/* required fields */
$incoming['type'] = 'auth';
$incoming['amount'] =  $this->request->data['Transaction']['amount'];
$incoming['ccexp'] = $this->request->data['Transaction']['ccexp'];
$incoming['ccnumber'] = $this->request->data['Transaction']['ccnumber'];

/* optional fields */
$incoming['payment'] = 'creditcard';
$incoming['cvv'] = 'credit card cvv';
$incoming['orderdescription'] =  $this->request->data['Transaction']['orderdescription'];
$incoming['orderid'] = $this->request->data['Transaction']['orderid'];


$incoming['firstname'] = $this->request->data['Transaction']['firstname'];
$incoming['lastname'] = $this->request->data['Transaction']['lastname'];
$incoming['company'] = $this->request->data['Transaction']['company'];
$incoming['address1'] = $this->request->data['Transaction']['address1'];
$incoming['city'] = $this->request->data['Transaction']['city'];
$incoming['state'] = $this->request->data['Transaction']['state'];
$incoming['zip'] = $this->request->data['Transaction']['zip'];
$incoming['country'] = $this->request->data['Transaction']['country'];
$incoming['phone'] = $this->request->data['Transaction']['phone'];
$incoming['fax'] = $this->request->data['Transaction']['fax'];
$incoming['email'] = $this->request->data['Transaction']['email'];
		
		
$result_array = $this->Payscape->Auth($incoming);

```
### Example Response: Auth Success

```
Array
(
    [response] => 1
    [responsetext] => SUCCESS
    [authcode] => 123456			// returned by the API when Auth Transaction is successful	
    [transactionid] => 2114304708	// returned by the API when Auth Transaction is successful
    [avsresponse] => N
    [cvvresponse] => N
    [orderid] => 20140103081036TestAuthCC
    [type] => auth
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```
### Example Capture 
```
$incoming = array();
$incoming['type'] = 'capture';
$incoming['transactionid'] = 'transaction id of the auth transaction';
$incoming['amount'] = '100.00'; // may not exceed Authorized amount

$result_array = $this->Payscape->Capture($incoming);

```
### Example Response Capture Success 
```
Array
(
    [response] => 1
    [responsetext] => SUCCESS
    [authcode] => 123456
    [transactionid] => 2114503473
    [avsresponse] => 
    [cvvresponse] => 
    [orderid] => 20140103112556TestAuthCC
    [type] => capture
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```

### Example Credit transaction
```
$time = gmdate('YmdHis');

$incoming = array();
$incoming['type'] = 'credit;
$incoming['transactionid'] = 'sale transaction id';
$incoming['amount'] = 'sale amount';
$incoming['orderid'] = 'sale transactionid';
		
$incoming['time'] = $time;	
	
/* optional fields */		
$incoming['firstname'] = $transaction['transactions']['firstname'];
$incoming['lastname'] = $transaction['transactions']['lastname'];
$incoming['company'] = $transaction['transactions']['company'];
$incoming['address1'] = $transaction['transactions']['address1'];
$incoming['city'] = $transaction['transactions']['city'];
$incoming['state'] = $transaction['transactions']['state'];
$incoming['zip'] = $transaction['transactions']['zip'];
$incoming['country'] = $transaction['transactions']['country'];
$incoming['phone'] = $transaction['transactions']['phone'];
$incoming['fax'] = $transaction['transactions']['fax'];
$incoming['email'] = $transaction['transactions']['email'];
```
### Example Response Credit Success
```
Array
(
    [response] => 1
    [responsetext] => SUCCESS
    [authcode] => 123456
    [transactionid] => 2114517479
    [avsresponse] => N
    [cvvresponse] => N
    [orderid] => 20140103113440Test
    [type] => sale
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```
### Example Validation transaction
```
$incoming = array();
$incoming['type'] = 'validate';
$incoming['ccexp'] = 'credit card expiration date';
$incoming['ccnumber'] = 'credit card number';

// optional fields
$incoming['cvv'] = 'credit card cvv';
		
$incoming['firstname'] = $this->request->data['Transaction']['firstname'];
$incoming['lastname'] = $this->request->data['Transaction']['lastname'];
$incoming['company'] = $this->request->data['Transaction']['company'];
$incoming['address1'] = $this->request->data['Transaction']['address1'];
$incoming['city'] = $this->request->data['Transaction']['city'];
$incoming['state'] = $this->request->data['Transaction']['state'];
$incoming['zip'] = $this->request->data['Transaction']['zip'];
$incoming['country'] = $this->request->data['Transaction']['country'];
$incoming['phone'] = $this->request->data['Transaction']['phone'];
$incoming['fax'] = $this->request->data['Transaction']['fax'];
$incoming['email'] = $this->request->data['Transaction']['email'];

$result_array = $this->Payscape->ValidateCreditCard($incoming);
```
### Example Response Validate Success
```
Array
(
    [response] => 1
    [responsetext] => SUCCESS
    [authcode] => 123456
    [transactionid] => 2117482337
    [avsresponse] => N
    [cvvresponse] => N
    [orderid] => 20140106083022Test
    [type] => sale
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```
### Example Refund transaction
```
$incoming = array();
$incoming['type'] = 'refund';
$incoming['transactionid'] = 'sale transaction id';
$incoming['amount'] = 'required only if refund is less than the origianl sale transaction amount';

$result_array = $this->Payscape->Refund($incoming);

```
### Example Response Refund Success
```
Array
(
    [response] => 1
    [responsetext] => SUCCESS
    [authcode] => 
    [transactionid] => 2114491871
    [avsresponse] => 
    [cvvresponse] => 
    [orderid] => 20131230143848Test
    [type] => refund
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```
### Example Update transaction
```
incoming = array();
$incoming['type'] = 'refund;
$incoming['transactionid'] = 'sale transaction id';
$incoming['shipping_carrier'] = 'shipping_carrier';
$incoming['tracking_number'] = 'shipping carrier tracking_number';
		
$result_array = $this->Payscape->Update($incoming);

```
### Example Response Update Success
```
Array
(
    [response] => 1
    [responsetext] => 
    [authcode] => 123456
    [transactionid] => 2114757737
    [avsresponse] => 
    [cvvresponse] => 
    [orderid] => 20140103151413Test
    [type] => update
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```
### Example Void transaction
```
$incoming = array();
$incoming['type'] = $'void';
$incoming['transactionid'] = 'sale transaction id';
$incoming['amount'] = 'sale amount';

$result_array = $this->Payscape->Void($incoming);
```
### Example Response Void Success
```
Array
(
    [response] => 1
    [responsetext] => Transaction Void Successful
    [authcode] => 123456
    [transactionid] => 2110075690
    [avsresponse] => 
    [cvvresponse] => 
    [orderid] => 20131230143602Test
    [type] => void
    [response_code] => 100
    [merchant_defined_field_6] => 
    [merchant_defined_field_7] => 
    [customer_vault_id] => 
)
```

#### Last updated: 1/15/2014
	  
