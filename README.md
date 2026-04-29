📖 Pokédex Angular — Despliegue en Azure
Autor: Luis Rodriguez Zabala
Curso: Sistemas Distribuidos
Plataforma: Azure Static Web Apps
URL pública: https://black-sky-0eac8bf10.7.azurestaticapps.net/
Repositorio: https://github.com/TU_USUARIO/pokedex (actualiza con tu usuario real)

📌 Descripción del Proyecto
PokeDex es una aplicación web desarrollada con Angular que permite a los usuarios explorar diferentes especies de Pokémon, mostrando información detallada sobre sus características y habilidades. La aplicación consume la API RESTful PokéAPI para obtener los datos en tiempo real.
El proyecto fue desarrollado originalmente por Keiler Mora y puede verse en vivo en: https://keilermora.github.io/pokedex-angular/

☁️ Creación de la Cuenta en Azure for Students
Requisitos previos

Correo institucional universitario (.edu)
No se requiere tarjeta de crédito

Pasos para crear la cuenta
Paso 1 — Ingresar al portal de Azure for Students
Acceder a https://azure.microsoft.com/es-es/free/students y hacer clic en "Comenzar gratis".
Paso 2 — Iniciar sesión con cuenta institucional
Usar el correo universitario para autenticarse. Microsoft verificará automáticamente el estado estudiantil.
Paso 3 — Completar la verificación
Seguir el proceso de verificación estudiantil. Una vez aprobado, se otorgan $100 USD de crédito sin necesidad de tarjeta de crédito.
Paso 4 — Acceder al Portal de Azure
Ingresar a https://portal.azure.com con las credenciales recién creadas. El panel principal de Azure estará disponible con la suscripción Azure for Students activa.

🛠️ Tecnologías Utilizadas
TecnologíaVersiónPropósitoAngular12+Framework frontendNode.js16+Entorno de compilaciónPokéAPI RESTv2Fuente de datos RESTPokéAPI GraphQLv1betaFuente de datos GraphQLGoogle Fonts—Tipografía (Roboto)Azure Static Web AppsFree tierHosting en la nubeGitHub Actions—CI/CD automatizado

🔒 Seguridad
La aplicación implementa los siguientes encabezados HTTP de seguridad, configurados mediante el archivo staticwebapp.config.json:
EncabezadoPropósitoContent-Security-PolicyPreviene ataques XSS controlando todos los recursos cargadosStrict-Transport-SecurityObliga el uso de HTTPS en todas las conexionesX-Content-Type-OptionsEvita la detección automática de tipos MIMEX-Frame-OptionsImpide que la app sea embebida en iframes externosReferrer-PolicyMinimiza la fuga de información en cabeceras HTTPPermissions-PolicyRestringe acceso a APIs del navegador (cámara, micrófono, etc.)
Configuración final (staticwebapp.config.json)
json{
  "globalHeaders": {
    "Content-Security-Policy": "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; img-src 'self' data: blob: https://raw.githubusercontent.com https://pokeapi.co https://assets.pokemon.com; connect-src 'self' https://pokeapi.co https://beta.pokeapi.co; font-src 'self' https://fonts.gstatic.com; object-src 'none'; base-uri 'self'; form-action 'self';",
    "Strict-Transport-Security": "max-age=31536000; includeSubDomains; preload",
    "X-Content-Type-Options": "nosniff",
    "X-Frame-Options": "DENY",
    "Referrer-Policy": "no-referrer",
    "Permissions-Policy": "geolocation=(), microphone=(), camera=()"
  },
  "navigationFallback": {
    "rewrite": "/index.html"
  }
}
Resultado del escaneo en securityheaders.com: 🟢 A

Nota técnica: La calificación A+ requeriría eliminar 'unsafe-inline' del script-src. Sin embargo, Angular inyecta scripts inline en tiempo de ejecución, lo que hace técnicamente inviable quitar esta directiva sin modificar el código fuente del framework.


💭 Reflexión Técnica
¿Qué vulnerabilidades previenen los encabezados implementados?

Content-Security-Policy previene ataques XSS (Cross-Site Scripting), definiendo exactamente qué dominios pueden proveer scripts, estilos, imágenes y conexiones.
Strict-Transport-Security elimina el riesgo de ataques Man-in-the-Middle (MITM) forzando siempre HTTPS.
X-Frame-Options: DENY previene ataques de Clickjacking, donde sitios maliciosos embeben la app en iframes invisibles para engañar al usuario.
X-Content-Type-Options: nosniff evita que el navegador interprete archivos con tipos MIME incorrectos.
Referrer-Policy: no-referrer protege la privacidad del usuario evitando que la URL de origen se filtre al navegar hacia otros sitios.
Permissions-Policy restringe el acceso a APIs sensibles del navegador como cámara, micrófono y geolocalización.

¿Qué aprendí sobre la relación entre despliegue y seguridad web?
El despliegue no termina cuando la aplicación está en línea. Durante este proceso aprendí que la configuración de encabezados HTTP de seguridad es una capa fundamental de protección que muchos desarrolladores omiten. Con pocos cambios de configuración se puede pasar de una calificación C a una A, demostrando que la seguridad no requiere grandes esfuerzos si se aplica desde el inicio.
También aprendí que el archivo staticwebapp.config.json debe estar exactamente en la raíz del repositorio para que Azure lo detecte y aplique correctamente los headers. Un error de ubicación puede resultar en que los headers no se apliquen, bajando la calificación de seguridad.
¿Qué desafíos encontré en el proceso?
Desafío 1 — Node.js no reconocido en CMD:
Al intentar ejecutar npm install, el sistema mostraba que npm no era reconocido como comando. Se resolvió instalando Node.js LTS desde nodejs.org, cerrando el CMD y abriéndolo nuevamente.
Desafío 2 — Calificación C en securityheaders.com:
Al escanear la app por primera vez, la calificación fue C con los headers Content-Security-Policy, X-Frame-Options y Permissions-Policy en rojo. La causa fue que el archivo staticwebapp.config.json tenía un error de formato. Se resolvió eliminando el archivo y creándolo nuevamente con el contenido correcto, obteniendo calificación A.

📎 Referencias

Keiler Mora — Autor original del proyecto
PokéAPI — API RESTful de Pokémon
Azure Static Web Apps Documentation
securityheaders.com
OWASP — Security Headers
MDN — Content Security Policy
