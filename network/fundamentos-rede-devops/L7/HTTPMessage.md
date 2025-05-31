HTTP Message

GET /somedir/index.html HTTP/1.1
Host: tibursio.me
Connection: close
User-agent: Mozilla
Accept-language: br

Request Line:
Methed -> Ação que deve ser feita, GET, POST, PUT...

Headers lines:
host: aonde estou me conectando (balanceador de carga faz a regra baceada no host)


HTTP Response
HTTP/1.1 200 OK
Connection: close
Date: Tue, 2 Nov 2020 20:36 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 2Nov 2020 19:47 GMT
Content-lenth: 6821
Content-Type: Text/html

Body -> corpo da mensagem


