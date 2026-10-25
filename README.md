# frases

Colección de frases célebres y refranes en castellano, en un único fichero de texto plano pensado para consumirse desde un script: más de un centenar de líneas, sin base de datos ni API de por medio.

La idea es la más simple posible: un fichero, un formato, y cualquier herramienta básica (`curl`, `awk`, `sed`) para leerlo.

## Formato

El fichero se llama `frases`, está codificado en UTF-8 y usa saltos de línea `\n`. Cada línea es una frase:

```
frase|autor
```

Por ejemplo:

```
Lo bueno, si breve, dos veces bueno.|Baltasar Gracián
```

Reglas:

- Exactamente una barra vertical `|` por línea, que separa la frase del autor.
- Sin líneas vacías ni comentarios.
- Sin `|` dentro de la frase ni del autor.
- Sin caracteres de control ni saltos de línea de Windows (`\r\n`).
- Las comillas internas se escriben con «», para no chocar con las comillas dobles de los scripts que lo consuman.

## Validar

Comprueba que todas las líneas tienen el formato correcto (no imprime nada si está bien):

```
awk -F'|' 'NF!=2 || $1=="" || $2=="" { print "MAL línea " NR ": " $0; bad=1 } END { exit bad }' frases
```

Y busca frases repetidas:

```
cut -d'|' -f1 frases | sort | uniq -d
```

## Usar

Una frase al azar, solo con herramientas disponibles hasta en un router con BusyBox:

```
curl -sf https://raw.githubusercontent.com/<usuario>/<repo>/main/frases |
    awk 'BEGIN { srand() } rand() * NR < 1 { l = $0 } END { print l }'
```

El `awk` elige una línea con probabilidad uniforme sin cargar el fichero entero en memoria. La salida es `frase|autor`; en un shell POSIX se separa con `${linea%|*}` (la frase) y `${linea##*|}` (el autor).

Sustituye `<usuario>` y `<repo>` por los de este repositorio.

## Sobre las atribuciones

Se han puesto con el mejor criterio, pero esto es una colección personal, no un trabajo de erudición. Algunas frases son paráfrasis tradicionales de textos antiguos, otras son traducciones libres del original y alguna está citada de segunda mano. Si detectas una mala atribución, se agradece una corrección.
