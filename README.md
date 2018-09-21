-Se clono el repositorio de Bit
git clone git@bitbucket.org:orbisunt/orbis-example-training.git
- Se agrega el nuevo origin remoto
git remote add origin2 git@github.com:euechi/orbis-example-training.git
- Buscamos el archivo mas pesado (se ordena por le tercer campo y se sacan solo los 3 primeros)
git verify-pack -v .git/objects/pack/pack-e121f70e46f7ec3df6d405dfb4895762265c6af7.idx | sort -k 3 -n | tail -3
- Obtenemos el nombre del archivo en base a su hash
git rev-list --objects --all | grep cfc3322ec593c88d0e4d68c312de3583ca741041
- Obtenemos los commits en donde se encuentra el archivo
git log --oneline --branches -- sc.16.tar.gz
-Se elimina el archivo de cada commit desde el primer commit en donde se encontraba el archivo
git filter-branch --index-filter \ 'git rm --cached -ignore-unmatch sc.16.tar.gz' -- f3a6dc7^..
-Se elimina el archivo de las referencias de los registros
rm -Rf .git/refs/original
rm -Rf .git/logs/
-Se ejecuta el garbage collector
git gc
-Se sube el archivo al nuevo repositorio remoto
git push origin2 master