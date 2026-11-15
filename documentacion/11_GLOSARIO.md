# 📖 11 · Glosario

Para leer el resto de la documentación sin conocimientos de astronomía. Basado en el glosario de `AGENTS.md` del repositorio oficial.

| Término | Qué es |
|---|---|
| **A.R. / RA** (ascensión recta) | Una de las dos coordenadas del cielo. Se da en horas (`08h55m10.831s`) o en grados (0–360). **Hay un factor 15 entre horas y grados; confundirlos es el error más común.** |
| **Dec** (declinación) | La otra coordenada, en grados de −90 a +90 |
| **ICRS / J2000** | Sistema de referencia de las coordenadas. J2000 es la época a la que están referidas muchas posiciones de catálogo |
| **Época** | Una observación en un instante dado. El «videíto» es la misma zona del cielo en distintas épocas |
| **Apuntado** | Una posición del telescopio. Cada posición cae en 2 archivos (pares D1+D4, D2+D5, D3+D6) |
| **Visita** | Un paso de SPHEREx por una zona, con varias exposiciones en pocos días |
| **Barrido** | Una pasada completa del cielo (1–6 semanas); se repite cada ~6 meses. **Un barrido no es una época** |
| **Movimiento propio** | Desplazamiento aparente de una estrella en el cielo, en milisegundos de arco por año (mas/yr) |
| **Paralaje** | Bamboleo aparente por el movimiento de la Tierra alrededor del Sol; período de un año; da la distancia |
| **Enana marrón** | Objeto entre planeta y estrella. Frías, rojas y brillantes en infrarrojo: lo que SPHEREx ve bien y lo que buscan los voluntarios de Backyard Worlds |
| **Infrarrojo cercano** | Luz de 0,75 a 5 µm, justo más larga que la visible. Atraviesa mejor el polvo |
| **Banda (D1–D6)** | Cada uno de los seis detectores de SPHEREx, con su rango de longitudes de onda |
| **LVF** (filtro lineal variable) | El truco óptico de SPHEREx: la longitud de onda cambia a lo largo de un eje del detector, así que *dónde cae* una fuente determina *a qué λ* se la mide |
| **λ** (longitud de onda) | Se mide en micrómetros (µm). Dos imágenes a λ distintas no son comparables |
| **FITS / MEF** | Formato estándar de imagen astronómica. Un MEF es un FITS con varias extensiones (imagen, varianza, máscara…) en el mismo archivo |
| **WCS** | La información del encabezado FITS que traduce píxel ↔ coordenada del cielo. `WCS-WAVE` hace lo mismo para la longitud de onda |
| **Cutout** (recorte) | Un trozo de una imagen grande alrededor de una posición |
| **HiPS** | Formato de teselas jerárquicas del CDS que permite navegar todo el cielo en el navegador; es lo que consume Aladin Lite |
| **SIA2** | *Simple Image Access v2*: servicio estándar (IVOA) para preguntar qué imágenes cubren una posición |
| **IRSA / IPAC** | Archivo de ciencia infrarroja de la NASA, en Caltech, que publica los datos de SPHEREx |
| **DR1 / QR3** | Versiones de datos de SPHEREx (*Data Release* 1 y *Quick Release* 3, según la nomenclatura de IRSA) |
| **ZODI** | Luz zodiacal: el brillo difuso del polvo del sistema solar, que se resta de cada imagen |
| **FLAGS** | Máscara de calidad por píxel, con un bit por tipo de problema |
| **Saturación** | Cuando una estrella es tan brillante que satura el detector; su núcleo queda enmascarado y la resta puede mostrar un movimiento falso |
| **PSF** | La forma en que el telescopio dibuja una fuente puntual. En SPHEREx es de 7,5–8,7″ |
| **Resta** | Imagen B − imagen A. Rojo = brillo que aparece; azul = brillo que estaba. Un par rojo/azul que se separa = movimiento |
| **Apilar** (*co-add*) | Sumar varias exposiciones para ganar señal frente al ruido. Aquí se usa la mediana o la suma ponderada por 1/VARIANCE |
| **Huella espectral** | El brillo de un objeto en cada banda, ordenado por λ: de qué está hecho |
| **Aladin Lite** | Visor de cielo del CDS (Estrasburgo) que corre en el navegador |
| **SIMBAD, Gaia, CatWISE, 2MASS, AllWISE** | Catálogos y cartografías de otras misiones usados como referencia |
| **Spherix** | La mascota del visor: guía y chat de ayuda |
| **Prisma** | La vista multibanda del visor (color traducido y huella espectral) |
| **PWA** | Aplicación web instalable: se agrega a la pantalla de inicio y abre sin barra del navegador |
| **Tauri / Capacitor** | Herramientas que envuelven una web en una app de escritorio (Tauri) o de Android (Capacitor) |
| **SHA-256** | Huella de un archivo: si dos archivos tienen la misma, son idénticos. Sirve para comprobar una descarga |
