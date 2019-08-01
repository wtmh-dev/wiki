---
title: shell
description: shell & bash
published: true
date: 2019-08-01T17:21:14.274Z
tags: 
---

# Header

### read

```
#!/bin/bash
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

```

```

```

```
