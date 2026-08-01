# Webinars Open Cluster Tech

Sitio público de inscripción y confirmación de los webinars de OpenCluster Tech
para equipos PIE, escuelas especiales y profesionales del área de educación.

**URL pública:** https://webinar.opencluster-tech.cl

---

## Cómo se publica

GitHub Pages, desde la rama `main`, carpeta raíz. El dominio propio lo define el
archivo `CNAME`. **Un push a `main` publica el sitio** (tarda un par de minutos);
no hay build ni GitHub Actions de por medio.

> Ojo: en GitHub Pages un dominio propio pertenece a **un solo repositorio**. Si
> alguna vez se mueve el sitio a otro repo, hay que quitar el dominio de aquí
> antes de asignarlo allá, y llevarse `confirmar.html` y `assets/` — los correos
> ya enviados apuntan a esas rutas exactas.

## Estructura

| Archivo | Webinar / uso | Estado |
|---|---|---|
| `index.html` | PIE Región del Maule — evento 24-jul-2026 | Realizado |
| `biobio-nuble.html` | PIE Biobío y Ñuble — evento **14-ago-2026, 16:00**. Un solo formulario para las dos regiones | Campaña en preparación |
| `biobio.html`, `nuble.html` | Redirecciones a `biobio-nuble.html?region=…`. Se mantienen para que los enlaces ya compartidos sigan funcionando | No editar |
| `metropolitana.html` | PIE Región Metropolitana | Sondeo, sin fecha definida |
| `confirmar.html` | Confirmación de asistencia. Adaptada a Biobío y Ñuble el 29-jul-2026 | **En uso por los correos** |
| `assets/` | Imágenes del sitio **y de los correos** | **En uso por los correos** |
| `CNAME` | Dominio propio de GitHub Pages | — |
| `encuesta_seleccion_horario_ORIGINAL.html` | Parece un respaldo de una versión anterior. No se verificó si algo lo usa. | Revisar antes de borrar |

### Dos cosas que NO hay que mover ni renombrar

- **`confirmar.html`** — el botón "Confirmar mi asistencia" de todos los correos
  apunta aquí, con los datos precargados en la URL. Si cambia de ruta, los
  correos ya enviados dejan de funcionar.
- **`assets/`** — el backend descarga estas imágenes por URL para incrustarlas en
  los correos. Si una imagen no está en su ruta, ese envío falla.

## Backend

Las inscripciones y confirmaciones van por POST a un **Google Apps Script**, que
las guarda en la planilla "Webinars OpenCluster" (una pestaña por webinar) y
envía los correos. El proyecto de Apps Script vive fuera de este repositorio.

Cada página llama a su propia acción del backend:

| Página | Acción | Pestaña de la planilla |
|---|---|---|
| `index.html` | `guardar_inscripcion_webinar` | `PIE_Maule_2026` |
| `biobio-nuble.html` | `guardar_inscripcion_bn` | `PIE_Biobio_Nuble_2026` |
| `metropolitana.html` | `guardar_inscripcion_rm` | `PIE_RM_2026` |

Biobío y Ñuble comparten webinar, pestaña y **formulario**. La región es un campo
del formulario, no se deduce de la página: el enlace de cada correo trae
`?region=biobio` o `?region=nuble` para dejarla premarcada, y quien recibió la
invitación reenviada puede cambiarla (incluso a "Otra"). Las comunas que se
sugieren se filtran según la región marcada.

Las acciones `guardar_inscripcion_biobio` y `guardar_inscripcion_nuble` siguen
existiendo en el backend por si alguien tiene la página anterior en caché.

## Medición

Las páginas leen `?src=...` (o `?utm_source=...` como respaldo) de la URL, lo
guardan junto a la inscripción y registran la visita en el backend. Así se puede
comparar cuánta gente **abrió** la página contra cuánta se **inscribió**, por
canal. El correo masivo de Biobío y Ñuble usa la etiqueta
`correo-presentacion-975`.
