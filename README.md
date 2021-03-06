# restmv

## Overview
The purpose of this project is to demonstrate using rest to do normal
pick basic functions.  This is mostly right now a demonstration project
around functionality.  This type of function should not be deployed
into production at this point.  Security items need to be implemented first

### restmv

Main router.  Will use both url options and a optional payload to define
actions.  

### Main options

Propose common payload feature to define environment items.  The idea
here is you could define running as a different emulation, different
account, etc.  Since each rest call is really a seperate call this is
possible.  

### Security
Security should be jwt tokens.  An init function (connect) would be called
to generate the token.  The token will then be passed on each future call

### Record Formats
Proposing to use a normal 3 level nested object to represent pick data. The
primary issue with this is around a multi-value that only has one value. At
this point it will be generated as a non-multivalue item.  When you change
a single to a multi-value with objects you would have to pull out your single
value, add an array, and then put your value back in.  Recommend building
platform libraries to do this.

In addition to keep pick attributes matched up we are using attribute 0
as a meta data field.  This will keep fields matching up.

```
Sample pick record
ED CUSTOMERS 12345
1>SMITH
2>JOHN
3>MOBILE]HOME]FAX
4>111-222-3333]111-222-3334]111-222-3335
5>5-1]5-2-1\5-2-2
```
```
In json
{ "rawdata": [
   "12345",     * NOTICE 0 WAS USED FOR ID
   "SMITH",     * RECORD<1>
   "JOHN",      * RECORD<2>
   [ "3",       * NESTED ARRAY, USE 0 TO SHOW WHAT ATTRIBUTE
     "MOBILE",  * RECORD<3,1>
     "HOME",    * RECORD<3,2>
     "FAX"      * RECORD<3,3>
   ],
   [ "4",        * NESTED ARRAY FOR 4
     "111-222-3333",  * RECORD<4,1>
     "111-222-3334,   * RECORD<4,2>
     "111-222-3335"   * RECORD<4,3>
   ],
   [ "5",         * NESTED ARRAY FOR 5
     "5-1",       * 5-1
     [ "5-2",     * NESTED ARRAY FOR 5-2 (SUBVALUES)
       "5-2-1",   * 5-2-2
       "5-2-2"    * 5-2-2
     ]
   ]
]
}

```

### Alternative Record Format - Pick Multi Value

Rest has no set rules on what the actual body data is. In addition, jSON can store pick @am, @vm and @svm just fine within a jSON payload.  An alternative is to build out a generic pick class for different platforms that would give you functions to deal with a Pick Dynamic record.  This module would have to be ported to any language that wishes to use the pick Dynamic functions.  The needed functions would need to be

 * consume pick record
 * export pick record
 * extract data
 * insert data
 * delete data
 * count @am, @vm, @svm
 * locate
 * optional - tojson
 * optional - fromjson
 
Other pick functions we could include in the library for local processing

 * oconv (numeric, date, time)
 * iconv (numeric, date, time)

Languages Needed

  * java (we have multiple versions available)
  * c# (we have multiple versions available)
  * javascript (MVExtensions has a version for node that should work)
  * Python (needs to be made)
  
### READ

../RESTMV/READ/FILE/ITEM

Reads a record and returns as a json array

* Locking...

### EXECUTE

../RESTMV/EXECUTE/CMND

Executes command.  Returns as a array (cr/@am to array element)

Needs to be highly expanded to handle

* Data statements
* returning
* errors
* encoding options

### WRITE

../RESTMV/WRITE/FILE/ITEM

* Data submitted as a json array
* locking

### DELETE

../RESTMV/DELETE/FILE/ITEM

### CALL

../RESTMV/CALL/SUBROUTINE/P1/P2/P3

* Best to be using a json payload
* Need to return all vars as json response

### ICONV

../RESTMV/ICONV/DATA/CONV

### OCONV

../RESTMV/OCONV/DATA/CONV
