---
title: shell
description: shell & bash
published: true
date: 2019-08-01T17:28:45.092Z
tags: 
---

# Header

**Listado de capítulos**

Estos son todos los capítulos del tutorial [Scripts en Bash](https://www.atareao.es/tutorial/scripts-en-bash/),

*   [1.\- Variables en Bash](https://www.atareao.es/tutorial/scripts-en-bash/variables-en-bash/)
*   [2.\- Condicionales en Bash](https://www.atareao.es/tutorial/scripts-en-bash/condicionales-en-bash/)
*   [3.\- Bucles en Bash](https://www.atareao.es/tutorial/scripts-en-bash/bucles-en-bash/)
*   [4.\- Arrays en Bash](https://www.atareao.es/tutorial/scripts-en-bash/arrays-en-bash/)
*   [5.\- Diccionarios en Bash](https://www.atareao.es/tutorial/scripts-en-bash/diccionarios-en-bash/)
*   [6.\- Funciones en Bash](https://www.atareao.es/tutorial/scripts-en-bash/funciones-en-bash/)
*   [7.\- Matemáticas en Bash](https://www.atareao.es/tutorial/scripts-en-bash/matematicas-en-bash/)
*   8.\- Preguntar al usuario en Bash
*   [9.\- Una ayuda para tus scripts](https://www.atareao.es/tutorial/scripts-en-bash/una-ayuda-para-tus-scripts/)


### read

```
#!/bin/sh
echo Introduce tu nombre
read nombre
echo Tu nombre es $nombre
```


```
#!/bin/bash
read -p "Dime tu nombre: " nombre
if [ "$nombre" != "" ]
then
    echo "Hola $nombre"
else
    echo "Pues si no me quires decir tu nombre peor para ti"
fi
```


```
#!/bin/bash
echo Dime tres nombres
read -a nombres
for i in ${nombres[@]}
do
    echo $i
done
```


```
#!/bin/bash
read -t 2 -p "Dime tu nombre: " nombre
if [ -z $nombre ]
then
    echo
    echo "No me has dicho tu nombre en menos de dos segundos"
else
    echo $nombre
fi
```

```
#!/bin/bash                                                                           
read -n 7 -p "Fruta de siete letras: " fruta
if [ "$fruta" == "plátano" ] 
then     
    echo
    echo Correcto
else
    echo
    echo "Error, la respuesta correcta era plátano"
fi
```

```
#!/bin/bash
read -d o -p "Fruta de siete letras: " fruta
if [ "$fruta" == "plátan" ]
then
    echo "Correcto"
else
    echo "Error, la respuesta correcta era plátano"
fi
```

```
#!/bin/bash
read -sp "Introduce la contraseña: " password
echo $password
```

---
---
---

```

```

```

```
