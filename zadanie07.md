BACKEND:prod  -  Total:
Total: 48 (HIGH: 48, CRITICAL: 0)

Z --ignore unfixed:
Total: 1 (HIGH: 1, CRITICAL: 0)

BACKEND:dev  -  Total:
Total: 48 (HIGH: 48, CRITICAL: 0)

Z --ignore unfixed
Total: 1 (HIGH: 1, CRITICAL: 0)

FRONTEND:prod  -  Total:
-
FRONTEND:przed  -  Total:
-
--pull nie zmienil wyniku, bo od zajac 13 nie wyszla nowa wersja obrazu bazowego albo nowa wersja nie latala zadnej podatnosci 

Ostatnia linia przed dodaniem tmpfs FileNotFoundError: [Errno 2] No usable temporary directory found in ['/tmp', '/var/tmp', '/usr/tmp', '/app']

docker compose exec backend touch /etc/test:
touch: cannot touch '/etc/test': Read-only file system

docker compose exec backend touch /tmp/test:

docker compose exec backend grep CapEff /proc/1/status:
CapEff: 0000000000000000

docker compose exec backend pytest:
================================================= test session starts ==================================================
platform linux -- Python 3.12.16, pytest-8.4.2, pluggy-1.7.0
rootdir: /app
collected 7 items

tests/test_check.py .......                                                                                      [100%]

================================================== 7 passed in 0.13s ===================================================

read_only - nikt nie zapisze nic w systemie plikow kontenera, nawet root
cap_drop ALL - proces traci uprawnienia jadra
no-new-privileges: blokuje zdobycie uprawnien przez pliki setuid/setgid, np. su.
read_only a USER: USER okreala, kim jest proces, i dziala przez uprawnienia plikow. appuser dalej moze pisac tam, gdzie ma prawa, a root wszedzie. read_only to cecha systemu plikow, ktora blokuje zapis kazdemu, takze rootowi.


docker history sekret-demo:plik :
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
<missing>      6 minutes ago   COPY klucz.txt /tmp/klucz.txt # buildkit        12.3kB    buildkit.dockerfile.v0

Get-ChildItem obraz/blobs/sha256 | Where-Object { tar -tf $_.FullName 2>$null | Select-String '^tmp/klucz.txt$' } | ForEach-Object { tar -xOf $_.FullName tmp/klucz.txt }:
tajny-klucz-dawid


kazda instrukcja tworzy osobna warstwe. RUN rm tylko dodaje w nowej warstwie znacznik usuniety, a oryginal zostaje w warstwie z COPY. Klucz trzeba bylo przekazac przez docker build --secret i RUN --mount=type=secret.