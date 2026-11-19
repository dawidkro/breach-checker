Wersja Trivy 0.76.0 
data skanu 19.11.2026r
BACKEND:prod  -  212 MB
Podatnosci z ignore unfix - 1
Podanosci bez ignore unfixed - 48
BACKEND:dev  -  230 MB
Podatnosci z ignore unfixed - 1
Podanosci bez ignore unfixed - 48
FRONTEND:prod  -  93 MB
Podatnosci z ignore unfixed - 0
Podatnosci bez ignore unfixed - 0
FRONTEND:przed  -  225 MB
Podatnosci z ignore unfixed - 0
Podatnosci bez ignore unfixed - 0


Zrobiono multi-stage we frontendzie usunal wszystkie podatnosci node-pkg, a --pull przebudowal obrazy na aktualnej bazie

NIe da sie naprawic podatnosci w debian w python:3.12-slim nie maja jeszcze poprawki w Debianie.

Kolejny skan co tydzien i po kazdej zmienie obrazu 