# SNOOPYO ACR

##DESCRIPTION
Guide of **SNOOPYO ACR (Access Control Rules)** for deployment and configure one node application.

Table of Contents
-----------------

1. [Rule Types](#rule-types)
	- [Application Based](#application-based)
	- [Routes Based](#routes-based)
	- [Sample](#sample)
2. [Rules Options](#rules-options)
	- [ACL](#acl)
	- [BLOCK](#block)
		- [TYPE](#type)
		- [PAYLOAD](#payload)
	- [REQ_LIST](#required-list)
		- [body](#body)
		- [query](#query)
		- [headers](#headers)
	- [BLIST](#black-list)
		- [body](#body)
		- [query](#query)
		- [headers](#headers)
3. [Priority](#priority)
4. [Examples](#priority)


### Rule Types
There are two types of rules which you can set.
	
### Application Based
Rules based on node **application** instance

### Routes Based
Rules based on node **application Routes** instance
##### Sample
	{
		POST:{
		  BLOCK:{
		  	TYPE:[1, 2],
		  	PAYLOAD:{/*body:true, params:true, query:true*/}
		  },
		  REQ_LIST:{
		  	query:[],
		  	body:[ 'line', 'skipLog', 'query', 'appid' ],
		  	headers:['snoopyo-apikey', 'authorization']
		  },
		  ACL:['write','admin','listind'],
		  BLIST:{
		  	body:{ skipLog:[true] }
		  	, query:{}
		  	, params:  {}
		  	, headers: {}
		  }
		}
		, GET:{ ... }
	}

Rules Options
-------------

### `ACL` (Access Control List)

`ACL` option defines that which rules set of token can access this API. Its value is an array of strings.

Type : Array or undefined/null.

Default : According to [Priority](#priority)


__Examples__

1.  _value_: `undefined`,`null`

  _valid_: `any`

2.  _value_: `['admin', 'search']`

  _valid_: `['admin']`, `['search']`, `['admin', 'search']`
  
  _invalid_: `['read']`, `['write']`, `['any', '...']`

**NOTE** ACL will apply **only** if it defined



### `BLOCK`

`BLOCK` Reject that request which will meet this criteria. And have two properties(options) `TYPE` and `PAYLOAD`. And selection will be According to [Priority](#priority)

Type : Object.

Structure : {`TYPE`: [Single criteria](#single-criteria) , `PAYLOAD`:{ [Payload criteria](#payload-criteria) }}

Default : `undefined`

__Examples__




### Types of criteria
##### Single criteria
- `Integer`
	
	Exact match of integer
	
	_valid_: `1`, `2`, any integer
	
	_invalid_: `a`, `b`, any non-integer

- `String`
	
	Exact match of string
	
	_valid_: `"string"`, any string
	
	_invalid_: `a`, `b`, any non-integer

- `RegExp`
	
	Test with [RegExp][RegExp]
	
__Examples__

1.	_value_: `string**`

		_valid_: All strings starts with `string`

		_invalid_: All strings do not start with `string`
	


##### Payload criteria
This will be **Always an Object**. and have these properties

- body
- query
- params
- headers

Priority
-------------
Here is priority of ACR.

##### Sample
	// POST method ACR for api (`'/single/route(api)/'`)
	{
		priority:4
		, POST:{priority:3}
		, '/single/route(api)/':{
			priority:2
			, POST:{ priority:1 }
		}
	}



[RegExp]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/RegExp
