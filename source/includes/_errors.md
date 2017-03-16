# Errors

The Exchange API uses the following codes in the [Error Object](#error-object):


Error Code | Meaning
---------- | -------
0101 | Authentication Error
0102 | Unknown System Error
0103 | Service temporarily unavailable
0104 | Operation not allowed
0201 | Exchange Account is Frozen
0202 | Exchange Account over accumulative limit for <day, week, month, year>
0203 | Transaction Amount over limit
0301 | Mandatory parameter missing: <parameter name>
0302 | Invalid value for parameter: <parameter name>
0303 | Invalid value for query parameter: <parameter name>
0304 | Invalid Account ID
0305 | Invalid Limit Order ID
0306 | Invalid Transaction ID
0307 | Invalid Notification ID
0308 | Document does not exist
0309 | Transaction ID already used
0310 | File Type not allowed
0311 | File too large
0401 | Account already exists
0402 | Document already exists
0403 | Limit Order already exists
0404 | Transaction already exists

<aside class="notice"><br>
Error codes beginning with <code>01**</code> are General Errors<br>
Error codes beginning with <code>02**</code> are Exchange Account Errors<br>
Error codes beginning with <code>03**</code> are Invalid Data Errors<br>
Error codes beginning with <code>04**</code> are Duplicate Errors</aside>


















