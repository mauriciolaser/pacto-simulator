# Pacto Simulator

Esta es una copia de Pacto Simulator, un juego de simulación política creado para la campaña del candidato al senado peruano, Gino Costa. 
La copia del juego disponible en el repositorio fue obtenida mediante scrapping de rutas públicas el día 13 de mayo de 2026. El juego ha sido archivado para fines de estudio académico.

## Antecedentes

El juego se alojó originalmente en: https://www.ginocosta.com/pactosimulator y fue creado por el equipo de Gino Costa. El candidato lo presentó en la red social formalmente conocida como Twitter mediante este mensaje en este post: https://x.com/CostaGino/status/2042658612523212928

> Con mi equipo desarrollamos Pacto Simulator para recordar lo que está en juego el 12 de abril. El reto: liderar el pacto mafioso, ampliar su influencia y sobrevivir a la justicia, la prensa independiente y la ciudadanía. Ser mafioso es fácil. Sobrevivir a una ciudadanía decidida a combatirte, no tanto.

<img width="596" height="682" alt="image" src="https://github.com/user-attachments/assets/b54f045d-d380-4018-ae1a-105f49f8277b" />

El juego está descrito en la página como: 

> ¿Cuánto duras manejando el pacto mafioso? Un juego sobre corrupción sistémica. Después pregúntate quién lo pelea en el Senado.

Asimismo, en los campos de metadata se describe así: 

> Maneja minería ilegal, narcotráfico y extorsión. Compra congresistas. Aprueba leyes de impunidad. ¿Cuánto sobrevives?

Se usa este banner para generar las vistas previas al compartir la página:

<img width="1200" height="630" alt="image" src="https://github.com/user-attachments/assets/759cebd8-2311-459d-92c2-5a23cc8b1dcb" />

## Scraping

El contenido del juego se scrapeó utilizando un script descrito en [scraper.md](https://github.com/mauriciolaser/pacto-simulator/blob/main/docs/scraper.md).

## Tecnología

Es un juego web, compatible con dispositivos móviles. El [archivo de entrada de la página](https://github.com/mauriciolaser/pacto-simulator/blob/main/pactosimulator/ginocosta.com/pactosimulator.html) contiene el total del código del proyecto (capas de presentación, lógica y estructura). A primera vista, tanto el código como los assets del proyecto han sido generados utilizando inteligencias artificales generativas.
