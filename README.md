# Plataforma de Exámenes Parciales
Universidad de Panamá · Centro Regional Universitario de Veraguas

Aplicación web para crear pruebas cortas y parciales integradores de varias
asignaturas, generar un código de acceso único por prueba, aplicar el
examen a los estudiantes con corrección automática y retroalimentación
inmediata, y consultar el informe de calificaciones — por prueba individual
o consolidado por asignatura. **Cada docente tiene su propia cuenta (correo
y contraseña) y solo ve sus propias asignaturas, pruebas e informes.**

> **Si nunca ha programado, no se preocupe.** Este proyecto ya viene con la
> base de datos configurada (el mismo proyecto de Firebase ya probado
> anteriormente). Solo falta un paso adicional obligatorio: activar el
> sistema de cuentas de docente dentro de Firebase (un solo interruptor).

## ⚠️ Paso obligatorio antes de usar: activar el inicio de sesión

A diferencia de la versión anterior (una sola clave compartida), ahora cada
docente inicia sesión con su propio correo y contraseña, o con su cuenta de
Google. Para que esto funcione, hay que **activar** ese sistema dentro de
su proyecto de Firebase (viene apagado por defecto):

1. Entre a **https://console.firebase.google.com** y abra su proyecto
   (el mismo de antes, "Plataforma-Examenes-Parciales").
2. En el menú de la izquierda, use la caja **"Buscar productos"** (arriba
   de todo) y escriba **Authentication**. Haga clic en el resultado.
   (El menú de Firebase cambia de vez en cuando de acomodo; el buscador
   siempre funciona sin importar dónde esté ubicado.)
3. Haga clic en **"Comenzar"** (o "Get started").
4. Va a ver una lista de "Proveedores de acceso". Haga clic en **"Correo
   electrónico/contraseña"**, active el interruptor y **"Guardar"**.

### Activar también "Entrar con Google" (recomendado)

Con esto sus docentes pueden entrar con un clic usando su cuenta de Gmail,
sin inventar otra contraseña más:

1. Todavía en Authentication → pestaña **"Método de acceso"**.
2. Haga clic en **"Agregar proveedor nuevo"** (o, si ve un aviso que dice
   "Se recomienda Acceder con Google", puede hacer clic directo en el
   botón **"Habilitar"** de ese aviso).
3. Elija **"Google"** de la lista.
4. Active el interruptor. Le va a pedir un "correo de asistencia del
   proyecto" — seleccione su propio correo de la lista.
5. Haga clic en **"Guardar"**.

### Autorizar su dominio de Netlify (necesario para Google)

Google exige que el sitio donde aparece el botón "Continuar con Google"
esté en una lista de dominios autorizados, o el botón dará un error.

1. Dentro de Authentication, vaya a la pestaña **"Configuración"**.
2. Busque la sección **"Dominios autorizados"**.
3. Haga clic en **"Agregar dominio"** y escriba la dirección de su sitio
   de Netlify **sin** `https://` ni barra al final — por ejemplo:
   `papaya-donut-779b6a.netlify.app`
4. Guarde.

Con esto, cualquier docente ya puede crear su cuenta desde la propia
aplicación (botón "Crear cuenta" o "Continuar con Google" en la pantalla
de acceso), sin que usted tenga que crearles la cuenta uno por uno.

## ⚠️ Paso obligatorio: actualizar las reglas de la base de datos

Las reglas de seguridad cambiaron para reflejar el sistema de cuentas
(antes cualquiera podía escribir cualquier cosa; ahora solo un docente con
sesión iniciada puede crear pruebas, y únicamente a nombre de su propia
cuenta).

1. Dentro de Firebase, vaya a **"Firestore Database" → pestaña "Reglas"**.
2. Borre el contenido actual y pegue el contenido del archivo
   `firestore.rules` de esta carpeta.
3. Haga clic en **"Publicar"**.

## Contenido del proyecto

```
index.html          → la aplicación COMPLETA en un solo archivo
                       (interfaz, lógica, y conexión a Firebase ya incluida)
firestore.rules      → reglas de seguridad actualizadas para el sistema de cuentas
netlify.toml          → configuración para publicar en Netlify sin build
vercel.json           → configuración para publicar en Vercel sin build
```

## Paso 1 — Crear el repositorio en GitHub

1. Entre a **https://github.com** y haga clic en **"New repository"**.
2. Póngale un nombre, por ejemplo `plataforma-examenes-parciales`. No
   marque ninguna opción adicional (README, .gitignore, licencia).
3. Haga clic en **"Create repository"**.
4. Busque el enlace **"uploading an existing file"**.
5. Arrastre los archivos de esta carpeta (`index.html`, `firestore.rules`,
   `netlify.toml`, `vercel.json`, `.gitignore`) a esa página.
6. Baje hasta el final y haga clic en **"Commit changes"**.

Si ya tenía un repositorio de una versión anterior, simplemente reemplace
el `index.html` existente por este (clic en el archivo → ícono de lápiz →
borrar todo → pegar el nuevo contenido → "Commit changes").

## Paso 2 — Publicar en Netlify

1. Entre a **https://app.netlify.com**.
2. **"Add new site" → "Import an existing project"**.
3. Conecte GitHub y seleccione el repositorio.
4. "Build command" vacío, "Publish directory" en `.`.
5. **"Deploy"**.

## Paso 3 — Asegurarse de que el sitio sea PÚBLICO

En **"Site configuration" → "Visitor access"**, confirme que esté en
**"Public"**. Si no, sus estudiantes no podrán abrir el enlace.

## Cómo confirmar que todo quedó bien

1. Abra la dirección de su sitio. No debe aparecer ninguna franja roja de
   "Modo local de prueba".
2. Entre a "Panel del docente" → pestaña **"Crear cuenta"** → complete su
   nombre, correo y una contraseña de al menos 6 caracteres.
3. Debería entrar directo a su panel, vacío (es su cuenta nueva).
4. Cree una prueba de prueba, cierre sesión, y vuelva a entrar con esa
   misma cuenta para confirmar que la prueba sigue ahí.
5. Pídale a alguien más (o abra una ventana de incógnito) que cree **otra**
   cuenta de docente distinta, y confirme que esa segunda cuenta **no ve**
   la prueba de la primera cuenta — cada quien ve solo lo suyo.

## Uso de la aplicación

### Cuentas de docente
- La primera vez, cada profesor hace clic en **"Crear cuenta"** dentro de
  "Panel del docente": nombre completo, correo y contraseña. Quedan con su
  propio panel, separado del de sus colegas.
- Las siguientes veces, usan **"Iniciar sesión"** con ese mismo correo y
  contraseña. La sesión se mantiene abierta entre visitas (no hay que
  volver a escribir la contraseña cada vez), hasta que hagan clic en
  **"Cerrar sesión"**.
- Si alguien olvida su contraseña, en la pantalla de inicio de sesión hay
  un enlace **"¿Olvidó su contraseña?"** que le envía un correo para
  restablecerla — sin que usted tenga que intervenir.

### Crear una prueba
- El nombre del docente se toma automáticamente de la cuenta con la que
  inició sesión (ya no se escribe a mano).
- Indique la **Asignatura**, el **módulo**, el tipo de prueba (Prueba
  Corta o Parcial Integrador) y las instrucciones.
- Suba el documento de preguntas (.docx o .txt) o agréguelas manualmente.
  Formato del documento:
  ```
  ASIGNATURA: Logística Internacional
  MODULO: Incoterms y Contratos Internacionales
  TIPO: prueba_corta
  INSTRUCCIONES: Lea cada pregunta y responda con cuidado.

  PREGUNTA: ¿Qué significa el incoterm FOB?
  TIPO_PREGUNTA: seleccion
  PUNTOS: 10
  OPCION: Free On Board
  OPCION: Free On Truck
  OPCION: Freight On Board
  RESPUESTA: Free On Board
  ```
  `TIPO_PREGUNTA` admite: `seleccion`, `vf` (verdadero/falso), `corta`. Si
  omite `PUNTOS`, el sistema reparte 100 puntos equitativamente.
- Al generar la prueba se crea un código de acceso único y un enlace
  directo para compartir con los estudiantes.
- **Disponibilidad (opcional):** puede definir una fecha de inicio y/o una
  fecha final. Fuera de ese rango, el estudiante ve un mensaje claro en vez
  del examen.
- **Control de tiempo (opcional):** elija una de tres opciones:
  - **Sin límite de tiempo** (como antes).
  - **Tiempo total para toda la prueba**: define los minutos totales.
    Aparece un cronómetro general. Al llegar a cero, la prueba **se envía
    automáticamente** con las respuestas que el estudiante tenga hasta ese
    momento (se califica normalmente lo que alcanzó a responder, no se
    anula).
  - **Tiempo individual por cada pregunta**: define los minutos
    permitidos por pregunta (el mismo límite se aplica a todas). Cada
    pregunta muestra su propio cronómetro; al agotarse, pasa
    automáticamente a la siguiente (quedando sin responder si no alcanzó),
    y en la última pregunta la prueba se envía sola.
  - En ambos casos de tiempo agotado, el estudiante ve un aviso
    indicándolo, y usted lo puede ver también en el informe de la prueba.
- **Mensaje de calificación al enviar:** al terminar la prueba (ya sea
  manualmente o porque se acabó el tiempo), el estudiante ve de inmediato
  un mensaje emergente con su calificación total sobre 100, además de
  verla en la pantalla de resultado con la retroalimentación completa.

### Editar, ver e imprimir una prueba ya creada
En el panel (y también dentro del informe de cada prueba) cada prueba tiene:
- **✏️ Editar**: permite cambiar fecha de inicio/fin (por ejemplo, **ampliar
  el plazo**), el control de tiempo, las instrucciones, el título, y corregir,
  agregar o quitar preguntas. **El código y el enlace no cambian**, así que
  no hay que volver a enviarlos a los estudiantes.
  - Si la prueba ya tiene estudiantes que la presentaron, la pantalla se lo
    avisa: cambiar fechas, tiempo o instrucciones es seguro; pero si cambia la
    respuesta correcta o los puntos de una pregunta, las calificaciones ya
    registradas **no se recalculan**.
  - Si los puntos no suman 100, la plataforma le pregunta antes de guardar.
- **👁 Ver / 🖨 Imprimir**: muestra la prueba tal como la verá el estudiante
  (con el encabezado institucional, espacios para nombre, cédula y fecha, y el
  valor total de 100 puntos), lista para imprimir en tamaño carta o guardar
  como PDF. La casilla **"Mostrar respuestas correctas"** genera la versión del
  docente con la clave; desmárquela para imprimir la versión del estudiante.

### Panel del docente — solo sus propias asignaturas
- Solo aparecen las pruebas creadas con su cuenta, agrupadas por
  asignatura, con un filtro para ver solo una a la vez.
- Cada asignatura tiene, además del informe de cada prueba individual, un
  botón **"📑 Informe consolidado de la asignatura"** con el promedio de
  cada estudiante en todas las pruebas de esa materia.

### Presentar una prueba (estudiante)
- Los estudiantes **no necesitan cuenta**. Entran con el código de acceso
  o el enlace directo, completan nombre/cédula/fecha, y responden.
- Preguntas y opciones en orden aleatorio por estudiante. Retroalimentación
  inmediata al finalizar, con opción de imprimir/guardar en PDF.
- Si sale de la pantalla del examen en curso, se anula automáticamente con
  nota 0 — solo para ese estudiante. Un único intento por prueba.

## Solución de problemas

**Al crear una cuenta me sale un error como "operation-not-allowed".**
Falta activar el proveedor de correo/contraseña en Firebase Authentication
— revise el primer paso obligatorio de este documento.

**Al hacer clic en "Continuar con Google" sale un error de dominio no
autorizado.**
Falta agregar la dirección de su sitio de Netlify en Authentication →
Configuración → Dominios autorizados (ver sección de Google arriba).

**Al crear/publicar una prueba me sale un error de permisos
("Missing or insufficient permissions").**
Las reglas de Firestore no se actualizaron. Revise el segundo paso
obligatorio (pegar y publicar el contenido de `firestore.rules`).

**Un docente no ve las pruebas que creó con la clave compartida antigua
(antes de este cambio).**
Es esperado: esas pruebas antiguas no están asociadas a ninguna cuenta
nueva, así que no aparecen en ningún panel personal. Si eran datos de
prueba/demostración no hay problema; si necesita recuperarlas dígamelo y
vemos cómo migrarlas.

**Veo una franja roja "Modo local de prueba" en el sitio publicado.**
Confirme que subió el `index.html` de esta carpeta sin modificarlo y que
Netlify terminó de publicar el último cambio.

**Mis estudiantes no pueden abrir el enlace del sitio.**
Revise el Paso 3 (Visitor access en "Public" en Netlify).

## Notas importantes

- Los estudiantes nunca inician sesión ni necesitan cuenta — solo el
  código de la prueba. Por eso, técnicamente, el contenido de una prueba
  (sus preguntas) es legible por cualquiera que tenga el enlace/código,
  igual que un formulario de Google compartido por enlace. Es un nivel de
  protección adecuado para uso académico normal.
- Cada docente ve y administra solo las pruebas creadas con su propia
  cuenta. Las reglas de la base de datos también impiden que un docente
  cree o modifique una prueba a nombre de otro.
- Si más adelante quiere un "superadministrador" que pueda ver todas las
  asignaturas de todos los docentes en un solo lugar, es una función
  adicional que se puede construir sobre esta base — avise si la necesita.
