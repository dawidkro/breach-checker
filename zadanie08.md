



0.430       L14 |        <h1>Breach Checker
0.430                    ^ Tag must be paired, missing: [ </h1> ], start tag match failed [ <h1> ] on line 14. (tag-pair)
https://github.com/dawidkro/breach-checker/actions/runs/45889229194


https://github.com/dawidkro/breach-checker/actions/runs/45889229194


https://github.com/dawidkro/breach-checker/actions/runs/45889816976



W tym uruchomieniu job verify pobiera z GHCR oba obrazy produkcyjne, backend i frontend, z tagiem skrotu tego samego commita i uruchamia projekt wylacznie z nich, wiec test dymny sprawdza dokladnie te obrazy, ktore trafilyby na produkcje. We wczesniejszej wersji pipeline'u z rejestru pochodzil tylko backend, a frontend byl budowany na runnerze z katalogu ./frontend, czyli weryfikowany byl inny obraz niz ten, ktory zostalby wdrozony (opublikowanego obrazu frontendu w ogole nie bylo).



https://github.com/dawidkro/breach-checker/actions/runs/45891367590



https://github.com/dawidkro/breach-checker/actions/runs/45891796512




Po nieudanym uruchomieniu latest backendu wskazywal ostatni obraz, ktory przeszedl weryfikacje (z commita przed zepsuciem), a nie zepsuty. Docker pull bez tagu oznacza latest. Wczesniej taka osoba dostalaby obraz, ktory nie odpowiada na zadne zapytanie, i nie wiedzialaby o tym. Zepsuty obraz nadal jest w rejestrze pod tagiem commita  przyda sie do analizy, ale nikt nie pobierze go przypadkiem.


PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker pull ghcr.io/dawidkro/breach-checker-backend:latest
latest: Pulling from dawidkro/breach-checker-backend
0a815fc54484: Pull complete
cb524fdce523: Pull complete
eab06a4ce8bf: Pull complete
c1d39acb9c4f: Pull complete
e8942684ba5a: Pull complete
Digest: sha256:157656b239291e43ace585b5539d5aa11e4707cce8dcfd6eb3eb29455311619c
Status: Downloaded newer image for ghcr.io/dawidkro/breach-checker-backend:latest
ghcr.io/dawidkro/breach-checker-backend:latest
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker pull ghcr.io/dawidkro/breach-checker-backend:3dd3c415c92389e7d41e77fd687558ecac1da6d0
3dd3c415c92389e7d41e77fd687558ecac1da6d0: Pulling from dawidkro/breach-checker-backend
ce77d32b4403: Pull complete
b09048842e30: Pull complete
a6ecfea7213c: Pull complete
c2780a7480f1: Pull complete
3b406fc1f859: Pull complete
Digest: sha256:d371bcb2eec654c5ecfb0e9d7f568af4297f9d5d34df28a7b3d06b52452dcf4d
Status: Downloaded newer image for ghcr.io/dawidkro/breach-checker-backend:3dd3c415c92389e7d41e77fd687558ecac1da6d0
ghcr.io/dawidkro/breach-checker-backend:3dd3c415c92389e7d41e77fd687558ecac1da6d0
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker images --digests ghcr.io/dawidkro/breach-checker-backend
REPOSITORY                                TAG                                        DIGEST                                                                    IMAGE ID       CREATED          SIZE
ghcr.io/dawidkro/breach-checker-backend   latest                                     sha256:157656b239291e43ace585b5539d5aa11e4707cce8dcfd6eb3eb29455311619c   157656b23929   6 minutes ago    182MB
ghcr.io/dawidkro/breach-checker-backend   3dd3c415c92389e7d41e77fd687558ecac1da6d0   sha256:d371bcb2eec654c5ecfb0e9d7f568af4297f9d5d34df28a7b3d06b52452dcf4d   d371bcb2eec6   13 minutes ago   182MB
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker push ghcr.io/dawidkro/breach-checker-backend:latest
The push refers to repository [ghcr.io/dawidkro/breach-checker-backend]
286d8c291daa: Layer already exists
30cf67d7101c: Layer already exists
e3e428093f5e: Layer already exists
83a14706aa6d: Layer already exists
cb524fdce523: Layer already exists
eab06a4ce8bf: Layer already exists
c1d39acb9c4f: Layer already exists
0a815fc54484: Layer already exists
e8942684ba5a: Layer already exists
latest: digest: sha256:157656b239291e43ace585b5539d5aa11e4707cce8dcfd6eb3eb29455311619c size: 403
PS C:\Users\Dawid\Desktop\labmw\breach-checker>



Ten sam digest = oba tagi to etykiety na tej samej zawartosci obrazu.
pull dziala, bo publiczny pakiet moze pobrac kazdy, bez uwierzytelnienia. push nie dziala, bo publikacja zawsze wymaga tokenu z prawem zapisu widocznosc zmienia tylko odczyt.
Token z write:packages z kroku 1 sluzyl do recznej publikacji obrazu (i do pobierania, gdy pakiet byl prywatny). W pipeline jego role przejal GITHUB_TOKEN.