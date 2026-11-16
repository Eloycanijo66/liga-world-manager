# Liga World Manager

Web de la Liga World Manager: 3 ligas de 20 managers de Futmondo (60 en total) que se enfrentan uno contra uno cada jornada. Se publica con GitHub Pages desde la rama `main`.

## Archivos

- `index.html`: la web completa en un solo archivo. Los datos van dentro, en `<script type="application/json" id="estado">`. No hay que compilar nada.

## Datos (`#estado`)

```json
{"version":1,"ligas":[{"id":"liga1","nombre":"1ª Liga Tajarina WMC 26-27",
  "equipos":["VÉJER BREMEN", "..."],
  "calendario":{"1":["0-1","2-3", "..."], "...": []},
  "resultados":{"1":{"goles":{"0":3,"1":3}}, "13":{"puntos":{"0":72}}}}]}
```

- `calendario[n]`: 10 partidos `"local-visitante"`, con los índices de `equipos`.
- `resultados[n].goles`: goles copiados de gesliga (jornadas 1 a 12).
- `resultados[n].puntos`: puntos de Futmondo; la web los convierte en goles. Si un equipo tiene `puntos`, mandan sobre `goles`.
- Dentro del JSON, `<` va escapado como `<` (la web lo regenera así).

## Reglas

- Canje de puntos a goles: menos de 50 puntos = 0 goles; desde 50, un gol más cada 9 puntos (`goles = floor((p-50)/9)+1`). 50–58 = 1, 59–67 = 2, …, 194–202 = 17.
- Victoria 3 puntos, empate 1. Desempate: diferencia de goles y luego goles a favor.
- Partidos aplazados de LaLiga: sus puntos NO cuentan nunca, ni cuando se aplazan ni cuando se juegan después. Una jornada vale con los puntos que había al acabar los partidos jugados en su fecha.
- Jornadas 1 a 12: resultados oficiales de gesliga. No recalcularlas con puntos de Futmondo.

## Actualizar una jornada

El dueño manda capturas de Futmondo (Clasificación → Jornadas → Jornada N, los 60 equipos con sus puntos). Los nombres en Futmondo no siempre son iguales a los de la web (p. ej. "QUILLO" = "REC-QUILLO", "DECKER" = "DECKER MV", "At.Rocaense" = "CLUB ATLETICO ROCAENSE", "salvatore" = "SALVADORE C.F", "NEGREIRA BOYS" = "NEGREIRAS TEAM", "cuki 2 c.f." = "CUKI CLUB", "CARNICA90 C.F" = "CARNICA F .C").

1. Leer los 60 puntos de las capturas y emparejarlos con los equipos de cada liga.
2. Escribir `resultados[n].puntos` en las 3 ligas (claves = índice del equipo como texto).
3. Comprobar que las 3 ligas tienen los 20 equipos de esa jornada y enseñar al dueño los resultados calculados antes de subir.
4. Commit y push a `main`. GitHub Pages actualiza la web en uno o dos minutos.

El dueño también puede apuntar puntos desde la propia web abriendo `…/#admin`: se descarga un `index.html` nuevo que habría que subir al repositorio.
