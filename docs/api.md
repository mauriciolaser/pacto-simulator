# API de `pactosimulator`

Este documento describe la API que consume el frontend de `pactosimulator`, el estado real observado del endpoint remoto y las implicancias para una reproducción local fiel.

## Resumen

El juego no carga JavaScript desde archivos externos: la lógica del frontend está inline en `pactosimulator.html`. Sin embargo, el leaderboard depende de un backend HTTP en `/api/leaderboard`.

A fecha `2026-05-13`, el endpoint remoto responde, pero no está plenamente operativo:

```json
{"scores":[],"error":"KV not configured"}
```

Eso significa que:

- La ruta existe.
- El frontend original espera usarla.
- El almacenamiento del backend remoto no está configurado.
- Un espejo estático del sitio no puede preservar el leaderboard solo con HTML, CSS y assets.

## Endpoint identificado

El frontend usa únicamente esta ruta:

- `GET /api/leaderboard`
- `POST /api/leaderboard`

Referencias en el frontend:

- `fetch('/api/leaderboard')` en [pactosimulator.html](C:/game-studies/pacto-simulator/pactosimulator/ginocosta.com/pactosimulator.html:3186)
- `fetch('/api/leaderboard', { method: 'POST', ... })` en [pactosimulator.html](C:/game-studies/pacto-simulator/pactosimulator/ginocosta.com/pactosimulator.html:3204)

## Fisonomia del API

### GET `/api/leaderboard`

Uso esperado por el frontend:

- Recuperar el ranking.
- Leer `data.scores`.
- Si falla, mostrar un error silencioso en consola y seguir sin leaderboard.

Respuesta observada hoy:

```json
{
  "scores": [],
  "error": "KV not configured"
}
```

Forma mínima que el frontend soporta correctamente:

```json
{
  "scores": [
    {
      "n": "Jugador",
      "m": 12,
      "r": 350000,
      "l": 3
    }
  ]
}
```

Campos usados por la UI:

- `n`: nombre o alias.
- `m`: meses sobrevividos.
- `r`: dinero robado.
- `l`: leyes aprobadas.

La UI muestra solo los primeros 20 scores.

### POST `/api/leaderboard`

Uso esperado por el frontend:

- Enviar el score actual del jugador en JSON.
- Esperar una respuesta JSON con `scores`.
- Opcionalmente leer `rank`.

Cuerpo enviado por el frontend:

```json
{
  "n": "Jugador",
  "m": 12,
  "r": 350000,
  "l": 3,
  "p": 2,
  "f": "fiscal"
}
```

Campos inferidos desde `window._runStats` y el submit:

- `n`: nombre o alias del jugador.
- `m`: meses vivos (`monthsAlive`).
- `r`: total robado (`totalStolen`).
- `l`: cantidad de leyes aprobadas.
- `p`: indice del partido final.
- `f`: clave del destino final o causa de caída.

Forma de respuesta esperada:

```json
{
  "scores": [
    {
      "n": "Jugador",
      "m": 12,
      "r": 350000,
      "l": 3
    }
  ],
  "rank": 1
}
```

Si `res.ok` es falso o no vuelve `scores`, el frontend cambia el texto del botón a un mensaje de error corto o a `Falló — reintentar`.

## Problema de preservacion

El sitio puede clonarse casi completo como artefacto estático, pero el leaderboard no se preserva exactamente solo con scraping porque depende de estado del servidor.

El problema concreto no es que falte la ruta, sino que el backend original hoy no tiene persistencia funcional (`KV not configured`).

En consecuencia:

- El frontend preservado sigue intentando llamar a `/api/leaderboard`.
- La UI del leaderboard puede abrirse.
- La carga o subida de scores no reproduce el comportamiento esperado del original.
- Para una reproducción fiel, hace falta reimplementar o emular ese backend.

## Implicancias para una replica local

Para mantener el comportamiento lo más igual posible, una replica local debería servir:

- Los archivos estáticos descargados.
- Un backend compatible con `GET /api/leaderboard` y `POST /api/leaderboard`.

Compatibilidad mínima recomendada:

- Aceptar y devolver JSON.
- Devolver siempre `scores` como array.
- Aceptar payload con `n`, `m`, `r`, `l`, `p`, `f`.
- Responder con `rank` tras un `POST`.

## Conclusión

Sí es posible preservar el frontend tal cual fue publicado, pero no el leaderboard funcional solo con scraping.

La fisonomia del API ya quedó identificada con suficiente detalle como para:

- documentar la dependencia real del juego;
- montar un mock fiel;
- o implementar un backend local compatible sin cambiar el HTML original.
