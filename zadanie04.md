PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker logs backend
Wczytano 89 hashy z /app/data/leaked-sha1.txt
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://172.19.0.2:5000
Press CTRL+C to quit
172.19.0.1 - - [30/Oct/2026 10:14:01] "GET /health HTTP/1.1" 200 -
PS C:\Users\Dawid\Desktop\labmw\breach-checker>



OK, UNREACHABLE, UNREACHABL
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker run --rm --network breach-net healthcheck-probe
>>
backend: OK
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker run --rm healthcheck-probe
>>
backend: UNREACHABLE
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker stop backend
>>
backend
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker run --rm --network breach-net healthcheck-probe
>>
>> docker run --rm --network breach-net healthcheck-probe
backend: UNREACHABLE
backend: UNREACHABLE
PS C:\Users\Dawid\Desktop\labmw\breach-checker>
PS C:\Users\Dawid\Desktop\labmw\breach-checker> ^C
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker start backend
backend


PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker rm -f backend
>>
backend
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker run -d --name backend --network breach-net -p 5000:5000 -v ./data/leaked-sha1.txt:/app/data/leaked-sha1.txt:ro breach-checker-backend:v1
>>
6987d3f0b1c49b7fb1b433cf9656e9c027bf090ce8d6c35f1fa875fd29566af0
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker exec backend cat /tmp/notatka.txt
cat: /tmp/notatka.txt: No such file or directory
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker run --rm -v breach-dane:/dane alpine sh -c "date >> /dane/historia.txt"
>>
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
Digest: sha256:be2656061b536c1883c4034790e21d0a09c5aaaf51968b89368a3c0371ea2c9a
Status: Downloaded newer image for alpine:latest
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker run --rm -v breach-dane:/dane alpine sh -c "date >> /dane/historia.txt; cat /dane/historia.txt"
>>
Fri Oct 30 10:20:34 UTC 2026
Fri Oct 30 10:20:40 UTC 2026
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker volume ls
DRIVER    VOLUME NAME
local     breach-dane
local     mw-projekt-restauracja_redisdata
local     projekt_redisdata
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker volume rm breach-dane