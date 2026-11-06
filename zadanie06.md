Testy z zajęć nie wykryły usunięcia `.lower()`, bo wszystkie wysyłały skróty zapisane małymi literami, dla których `.lower()` niczego nie zmienia - kod z tą metodą i bez niej zachowuje się dla nich identycznie. Błąd ujawnia dopiero test, który wysyła skrót wielkimi literami, czyli przypadek, dla którego ta linia w ogóle istnieje.

Wszystkie zadanie:
PS C:\Users\Student\Desktop\lab6\breach-checker> docker compose exec backend pytest -v
================================================= test session starts ==================================================
platform linux -- Python 3.12.16, pytest-8.4.2, pluggy-1.7.0 -- /usr/local/bin/python3.12
cachedir: .pytest_cache
rootdir: /app
collected 7 items

tests/test_check.py::test_leaked_hash_is_found PASSED                                                            [ 14%]
tests/test_check.py::test_unknown_hash_is_not_found PASSED                                                       [ 28%]
tests/test_check.py::test_missing_hash_field PASSED                                                              [ 42%]
tests/test_check.py::test_plaintext_password_is_rejected PASSED                                                  [ 57%]
tests/test_check.py::test_uppercase_hash_is_found PASSED                                                         [ 71%]
tests/test_check.py::test_hash_too_short_is_rejected PASSED                                                      [ 85%]
tests/test_check.py::test_non_hex_hash_is_rejected PASSED                                                        [100%]

PO modtydyfikacji:

PS C:\Users\Student\Desktop\lab6\breach-checker> docker compose exec backend pytest -v
================================================= test session starts ==================================================
platform linux -- Python 3.12.16, pytest-8.4.2, pluggy-1.7.0 -- /usr/local/bin/python3.12
cachedir: .pytest_cache
rootdir: /app
collected 7 items

tests/test_check.py::test_leaked_hash_is_found PASSED                                                            [ 14%]
tests/test_check.py::test_unknown_hash_is_not_found PASSED                                                       [ 28%]
tests/test_check.py::test_missing_hash_field PASSED                                                              [ 42%]
tests/test_check.py::test_plaintext_password_is_rejected PASSED                                                  [ 57%]
tests/test_check.py::test_uppercase_hash_is_found FAILED                                                         [ 71%]
tests/test_check.py::test_hash_too_short_is_rejected PASSED                                                      [ 85%]
tests/test_check.py::test_non_hex_hash_is_rejected PASSED                                                        [100%]

======================================================= FAILURES =======================================================
_____________________________________________ test_uppercase_hash_is_found _____________________________________________

client = <FlaskClient <Flask 'app'>>






>> '{"hash":"7c4a8d09ca3762af61e59520943dc26494f8941b"}' | curl.exe -s -o blad.html -w "HTTP %{http_code}\n" -X POST http://localhost:8080/api/check -H "Content-Type: application/json" -d '@-'
>>
HTTP 500



PO restarcie: 
PS C:\Users\Student\Desktop\lab6\breach-checker> docker compose ps -a
NAME                        IMAGE                     COMMAND                  SERVICE    CREATED          STATUS                              PORTS
breach-checker-backend-1    breach-checker-backend    "flask --app app run…"   backend    35 minutes ago   Exited (1) Less than a second ago
breach-checker-db-1         postgres:16-alpine        "docker-entrypoint.s…"   db         35 minutes ago   Exited (0) 2 minutes ago
breach-checker-frontend-1   breach-checker-frontend   "/docker-entrypoint.…"   frontend   35 minutes ago   Up 34 minutes                       0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
PS C:\Users\Student\Desktop\lab6\breach-checker> docker inspect --format '{{.State.Status}} {{.State.ExitCode}}' breach-checker-backend-1
exited 1



                                        PORÓWANIE AWARII Z BLOKADOĄ Z KROKU 4




PS C:\Users\Student\Desktop\lab6\breach-checker> curl.exe -I http://localhost:8080/nginx.conf
HTTP/1.1 404 Not Found
Server: nginx/1.31.8
Date: Fri, 06 Nov 2026 12:50:30 GMT
Content-Type: text/html
Content-Length: 153
Connection: keep-alive