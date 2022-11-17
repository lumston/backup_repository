# backup_repository

This Github Action uses 
User and Password from codecommit we use that secrets at Organization secret
When you create a user and password in AWS the password could be genereted with special characters and could break the autentication

If you have special characters replace those them for next:
https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters


Reserved characters after percent-encoding
␣	  !	  #	  $	  %	  &	  '	  (	  )	  *	  +	  ,	  /	  :	  ;	  =	  ?	  @	  [     ]
%20	%21	%23	%24	%25	%26	%27	%28	%29	%2A	%2B	%2C	%2F	%3A	%3B	%3D	%3F	%40	%5B	%5D

newline	              space	"	%	  -	  .	  <	  >	  \	  ^	  _	  `	  {	  |	  }	  ~	  £	      円
%0A or %0D or %0D%0A	%20	%22	%25	%2D	%2E	%3C	%3E	%5C	%5E	%5F	%60	%7B	%7C	%7D	%7E	%C2%A3	%E5%86%86

