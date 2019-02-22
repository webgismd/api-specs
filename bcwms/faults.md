# WMS Faults
Status code | Description | Notes
-------: | ---------------
200|	OK	|The request was successful
403|	Forbidden|	Often denotes a permissions mismatch
404|	Not Found|	Endpoint or resource was not at the indicated location
405|	Method Not Allowed|	Often denotes an endpoint accessed with an incorrect operation (for example, a GET request where a PUT/POST is indicated)
500|	Internal Server Error|	Often denotes a syntax error in the request
