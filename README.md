# backup_repository

This Github Action uses 
User and Password from codecommit we use that secrets at Organization secret
When you create a user and password in AWS the password could be genereted with special characters and could break the autentication

If you have special characters replace those them for next:
https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters


Reserved characters after percent-encoding
	  *	  +	  ,	  /	  :	  ;	  =	  ?	  @	  [     ]
	%2A	%2B	%2C	%2F	%3A	%3B	%3D	%3F	%40	%5B	%5D
  
|Header1 |Header2  | Header3|
--- | --- | ---|
|data1|data2|data3|

|␣ | ! | #| $ | % | & | '|  ( | ) | * | + | , | / | : | ; |  = | ? | @ | [ | ] |
---| --- |--- |--- | ---| ---| ---| ---| ---| ---| ---| ---| ---| ---| ---| ---| ---| ---| ---| ---|
%20  | %21 | %23 | %24 | %25 | %26 |  %27 | %28 | %29 | %2A| %2B| %2C | %2F | %3A | %3B | %3D | %3F | %40	| %5B|	%5D



