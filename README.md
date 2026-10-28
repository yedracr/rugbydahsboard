# Dashboard BIP — análisis de rugby (Ball In Play Logger)

Web estática (un solo `index.html`, sin dependencias, sin servidor, sin cuentas de IA). Funciona en cualquier navegador moderno.

## Publicar (GitHub Pages, gratis)
1. Crea una cuenta en github.com y un repositorio nuevo (p. ej. `rugby-dashboard`).
2. Sube `index.html`, `.nojekyll` y `README.md` (Add file → Upload files).
3. Settings → Pages → Source: *Deploy from a branch* → `main` / `(root)` → Save.
4. En 1–2 minutos tendrás la URL `https://TU-USUARIO.github.io/rugby-dashboard/`.

Alternativas: **Netlify** (arrastra la carpeta a app.netlify.com/drop) o **Vercel** (importa el repositorio, sin build). Nada más que configurar.

## Actualizar la web
Sustituye `index.html` en el repositorio (o vuelve a arrastrar la carpeta en Netlify). Tus datos no se pierden: están en tu navegador, no en el código.

## Uso en la temporada
- **Gestión de partidos** → arrastra el CSV tras cada jornada → revisa vista previa, fecha y rival → *Confirmar importación*. Nunca se sobrescribe el histórico; si detecta un duplicado eliges *actualizar*, *conservar ambas* o *cancelar*.
- **Copias de seguridad**: *Compartir dashboard* → *Copia de seguridad completa (JSON)*. Hazla después de cada jornada: los datos viven en el almacenamiento local del navegador (se pierden si borras datos del sitio, cambias de navegador o de dispositivo). Restaura con *Importar JSON* (fusionar o reemplazar).

## Compartir con otros entrenadores
1. **Enlace a la app**: la abre vacía en su navegador; no comparte tus datos.
2. **Archivo JSON**: se lo envías y lo importan en su propio navegador.
3. **Modo visitante (solo lectura)**: exporta *season.json* con los partidos que quieras compartir, súbelo al repositorio junto a `index.html` y comparte `https://…/?modo=visitante`. Los visitantes ven solo ese archivo y no pueden editar.
   - ⚠ No es seguridad real: cualquiera con la URL puede descargar `season.json`. No publiques lo que deba ser privado. Para permisos reales haría falta un backend con autenticación (Supabase/Firebase); la app ya separa datos, cálculo y vistas para añadirlo después.

## Fórmulas (verificadas con capturas de Review)
- Task view: Task time = Σ duración de tareas; Gap rest = Σ huecos entre tareas; Task % = Task time / (fin última tarea − inicio primera).
- BIP view: BIP Time = Σ duración BIP; Rest = ventana − BIP Time; BIP % = BIP Time / ventana, con ventana = fin de la última tarea con BIP − inicio de la primera tarea con BIP.
- Tabla de drills: Work = Σ BIP de la tarea; Rest = duración de la tarea − Work; BIP % = Work / duración; Rucks = `ruck_count` de la tarea; Total = duración de la tarea. Los segundos se truncan al mostrar (mm:ss).
- Histograma de duraciones: tramos (0,30], (30,60], (60,90], (90,120], (120,180], 181+.
- Rucks por BIP: `ruck_count` de cada BIP (tramos 0-3, 4-6, 7-9, 10-12, 13-15, 16+).
- Los registros task/bip/ruck se relacionan por `task_id`/`bip_id`/`ruck_id`; se ignoran ids repetidos.
- **No verificado**: tramos superiores de los histogramas, tramos de rucks por tarea y el cálculo exacto de Clock (aquí: último instante registrado, redondeado hacia arriba).
