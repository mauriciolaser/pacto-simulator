# Scraper de `pactosimulator`

Este documento explica cómo replicar localmente `https://ginocosta.com/pactosimulator` en `./pactosimulator` con reescritura offline.

## Requisitos

- Python 3.10+
- `pip`
- Dependencia base:

```bash
pip install requests
```

- Fallback dinámico opcional (recomendado):

```bash
pip install playwright
python -m playwright install chromium
```

## Ejecución

Desde la raíz del proyecto:

```bash
python scraper/scrape_pactosimulator.py
```

El script usa estos defaults:

- `--base-url https://ginocosta.com/pactosimulator`
- `--output-dir ./pactosimulator`
- `--max-passes 12`
- `--timeout 60`
- `--retries 5`
- `--use-playwright-fallback` activado

## Flags disponibles

```bash
python scraper/scrape_pactosimulator.py \
  --base-url https://ginocosta.com/pactosimulator \
  --output-dir ./pactosimulator \
  --max-passes 12 \
  --timeout 60 \
  --retries 5 \
  --use-playwright-fallback
```

Para desactivar el fallback dinámico:

```bash
python scraper/scrape_pactosimulator.py --no-use-playwright-fallback
```

## Salida esperada

Dentro de `./pactosimulator`:

- Descarga por host (`ginocosta.com/...`, `fonts.gstatic.com/...`, etc.)
- `download-ok.log`: mapa URL remota -> archivo local.
- `download-errors.log`: descargas fallidas con error.
- `all-asset-urls.txt`: inventario total de URLs detectadas.
- `missing-after-retry.txt`: URLs que siguen fallando luego de reintentos.

## Verificación rápida

- Confirmar existencia de `./pactosimulator/ginocosta.com/pactosimulator.html` (o equivalente).
- Abrir el HTML local principal y validar carga visual base.
- Revisar que `missing-after-retry.txt` esté vacío o con pocos casos no críticos.

## Troubleshooting

- Timeouts o red inestable:
  - Aumentar `--timeout` y/o `--retries`.
- Error de certificados SSL/proxy corporativo:
  - Probar en red sin inspección TLS o configurar entorno para certificados corporativos.
- Faltan assets dinámicos:
  - Instalar Playwright y Chromium.
  - Reejecutar con `--use-playwright-fallback` (default).
- Bloqueos puntuales de terceros:
  - Revisar `download-errors.log` y reintentar más tarde.
