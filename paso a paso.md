# Guía paso a paso para la Evaluación Parcial 3

## 1. Objetivo y orden recomendado

La evaluación consiste en planificar, ejecutar y explicar pruebas de calidad, seguridad y cumplimiento sobre Cuidarte+. Deben combinar:

1. **Diseño de pruebas:** definir qué comprobarán y con qué requisito se relaciona cada prueba.
2. **Análisis estático:** inspeccionar el código sin ejecutar la aplicación. En esta guía se usa SonarQube y el ESLint que ya incluye el frontend.
3. **Revisión manual y auditoría técnica:** contrastar el código, la configuración y el comportamiento con la ERS.
4. **Testing dinámico:** ejecutar la aplicación y comprobar casos funcionales y no funcionales.
5. **Documentación:** conservar resultados y evidencias reales, registrar incidentes y completar el informe.

**Empiecen por organizar y diseñar las pruebas**, no por instalar herramientas al azar. Definir primero los requisitos y los casos les permitirá saber qué deben probar, qué evidencia guardar y qué herramientas necesitan. Después ejecuten análisis estático y testing; al final completen el informe con resultados reales.

> SonarQube ayuda a encontrar problemas en el código, pero no reemplaza las pruebas funcionales, la revisión manual ni una auditoría legal. Un resultado sin alertas tampoco demuestra que el sistema sea seguro o que cumpla la ley.

## 2. Alcance de la evaluación

El proyecto Cuidarte+ incluye una aplicación React, una API Node.js/Express y una base de datos PostgreSQL. Sus funciones principales son autenticación, usuarios, pacientes, exámenes médicos, documentos y auditoría.

La ERS contiene:

- **17 requisitos funcionales:** RF-1.1 a RF-5.1.
- **20 requisitos no funcionales:** NFR-SEG-1 a NFR-SEG-9, NFR-PERF-1 a NFR-PERF-3, NFR-USAB-1 a NFR-USAB-4, NFR-DIS-1 a NFR-DIS-2 y NFR-COMPAT-1 a NFR-COMPAT-2.
- **3 reglas de negocio:** RB-1 a RB-3.
- Requisitos de interfaces y criterios de aceptación que también conviene incluir en la matriz.

La instrucción solicita cobertura del 100% de los requisitos de la ERS. La plantilla pide al menos cinco casos representativos, pero cinco casos por sí solos no cubren todos los requisitos. Un caso puede cubrir varios requisitos relacionados; asegúrense de que cada uno aparezca en la matriz.

La ERS tiene referencias que parecen genéricas o heredadas de otro sistema, como “Atlas” y menciones a clientes y contratos. Anoten la inconsistencia, indiquen cómo interpretaron el alcance y consulten al docente si no está claro. No cambien los requisitos silenciosamente.

## 3. Reglas para trabajar de forma segura

- Ejecuten las pruebas **solo en un entorno local autorizado** y sobre datos ficticios.
- No conecten SonarQube ni las pruebas a servicios de producción, datos reales de pacientes o sistemas de terceros.
- No publiquen la aplicación, la API, la base de datos ni SonarQube en una red accesible por otras personas.
- No incluyan contraseñas, tokens, datos personales ni documentos clínicos reales en el informe, capturas o historial de comandos.
- El README del proyecto avisa que el backend es intencionalmente vulnerable. Trátenlo como material de laboratorio; no reutilicen sus credenciales o secretos fuera del entorno local.
- Si una prueba no pudo ejecutarse, márquenla como **bloqueada** o **no ejecutada**, expliquen el motivo y no la cuenten como aprobada.

## 4. Preparar la carpeta y el equipo

1. Abran PowerShell o la terminal de VS Code.
2. Confirmen que Docker Desktop esté iniciado y que tengan Node.js/npm instalados:

   ```powershell
   docker --version
   docker compose version
   node --version
   npm --version
   ```

3. Sitúense en la carpeta del código:

   ```powershell
   Set-Location 'S:\1- Sergio\Cursos\Seguridad Y Calidad en Desarrollo de Software\EP2\CodigoFuenteB'
   ```

4. Revisen qué archivos de feedback de la evaluación anterior deben corregir y anoten los nombres de quienes ejecutarán cada prueba.
5. Creen una carpeta de trabajo para guardar capturas, exportaciones del análisis, resultados y una copia de la matriz de pruebas. No incluyan secretos ni datos reales.

## 5. Crear la matriz de cobertura y diseñar los casos

Antes de ejecutar pruebas, creen una matriz con una fila por cada requisito de la ERS:

| Requisito | ID de caso | Tipo de prueba | Estado | Evidencia / observaciones |
|---|---|---|---|---|
| RF-1.1 | CP-01 | Funcional: autenticación | Diseñado | Se completará al ejecutar |
| NFR-SEG-4 | CP-03 | Seguridad: autorización por rol | Diseñado | Se completará al ejecutar |
| RB-1 | CP-03 | Funcional: permisos del paciente | Diseñado | Se completará al ejecutar |
| NFR-PERF-1 | CP-08 | Rendimiento | Diseñado | Se completará al ejecutar |

Usen estados distintos para **diseñado**, **ejecutado**, **aprobado**, **fallido**, **bloqueado** y **no ejecutado**. No confundan cobertura diseñada con cobertura ejecutada.

Para cada caso documenten lo que pide la plantilla:

- ID y nombre.
- Requisito(s) ERS asociado(s).
- Descripción y precondiciones.
- Datos ficticios de entrada.
- Pasos reproducibles.
- Resultado esperado y criterio de aceptación.
- Tipo de prueba.
- Resultado real, fecha, ejecutor y evidencia, que se completan después de ejecutarlo.

Casos representativos para empezar:

| Caso | Qué comprobar | Requisito(s) de referencia | Tipo |
|---|---|---|---|
| CP-01 | Iniciar sesión con una cuenta ficticia válida y comprobar el resultado esperado. | RF-1.1 | Funcional / integración |
| CP-02 | Cerrar sesión y comprobar el comportamiento posterior de la sesión. | RF-1.2, NFR-SEG-9 | Funcional / seguridad |
| CP-03 | Verificar que el rol paciente solo pueda leer su información y no realizar operaciones de escritura. | RB-1, NFR-SEG-4, RF-4.1 | Seguridad / autorización |
| CP-04 | Comprobar las operaciones de examen que correspondan al rol y al requisito, incluida la confirmación antes de eliminar. | RF-3.1 a RF-3.3 | Funcional |
| CP-05 | Subir y descargar un documento ficticio permitido y verificar el resultado esperado. | RF-4.2 a RF-4.4 | Funcional / rendimiento |
| CP-06 | Comprobar que una acción crítica genere su registro de auditoría. | RF-5.1, NFR-SEG-6 | Funcional / auditoría |
| CP-07 | Revisar legibilidad, navegación por teclado y flujo en una pantalla móvil. | Requisitos de interfaz, NFR-USAB-1 a NFR-USAB-4, NFR-COMPAT-2 | Usabilidad / compatibilidad |
| CP-08 | Medir operaciones CRUD simples bajo condiciones anotadas y compararlas con el umbral de la ERS. | NFR-PERF-1 | Rendimiento |

Estos son ejemplos de diseño, **no resultados**. Ajusten precondiciones y resultados esperados a la implementación y a los criterios exactos de la ERS. Añadan casos hasta que todos los RF, NFR, RB y criterios aplicables aparezcan en la matriz.

## 6. Levantar el sistema de prueba

Desde `CodigoFuenteB`, inicien los servicios locales con Docker Compose:

```powershell
docker compose up -d --build
docker compose ps
```

Según el README y el archivo de Compose, los puertos por defecto son:

- Frontend: `http://localhost:3333`
- Backend/API: `http://localhost:4444`
- Swagger/OpenAPI: `http://localhost:4444/docs`
- PostgreSQL: puerto local `15442`

Comprueben que los servicios estén en ejecución y que la aplicación responda antes de empezar. Si el sistema no inicia, registren el bloqueo y resuélvanlo antes de contabilizar pruebas dinámicas. No borren volúmenes ni bases de datos como forma de “arreglar” el entorno sin confirmar antes qué información contienen.

## 7. Ejecutar análisis estático con SonarQube

### 7.1 Iniciar SonarQube local

SonarQube es opcional según la pauta, pero es una herramienta apropiada para el análisis estático que quieren documentar. Una forma sencilla de usarlo en un laboratorio local es Docker. Requiere recursos suficientes; la documentación de Sonar recomienda al menos 4 GB de memoria para análisis de JavaScript/TypeScript/CSS.

En PowerShell:

```powershell
docker run -d --name sonarqube-cuidarteplus -p 9000:9000 sonarqube:community
docker ps --filter "name=sonarqube-cuidarteplus"
```

La primera inicialización puede tardar. Abran `http://localhost:9000` cuando el servicio esté disponible. El acceso inicial documentado por SonarQube es `admin` / `admin`; cambien la contraseña si el sistema lo solicita y no la incluyan en el informe.

Si ya existe un contenedor con ese nombre, no creen otro a ciegas: revisen su estado y decidan si corresponde iniciarlo o usar otro nombre/puerto.

### 7.2 Instalar SonarScanner CLI en Windows

1. Descarguen la versión **Windows x64** de SonarScanner CLI desde la [documentación oficial de SonarScanner](https://docs.sonarsource.com/sonarqube-server/analyzing-source-code/scanners/sonarscanner/).
2. Extraigan el archivo en una ubicación local, por ejemplo `C:\tools\sonar-scanner`.
3. Agreguen temporalmente la carpeta `bin` del scanner al `PATH` de la sesión actual. Ajusten la ruta si extrajeron el archivo en otro lugar:

   ```powershell
   $env:Path += ';C:\tools\sonar-scanner\bin'
   sonar-scanner.bat --version
   ```

   Si PowerShell no encuentra el comando, localicen `sonar-scanner.bat` dentro de la carpeta extraída y ejecuten ese archivo por su ruta completa. Usen un paquete Windows x64 que incluya el runtime o sigan los requisitos de Java indicados por la documentación.

### 7.3 Crear un token personal

En la interfaz de SonarQube, inicien sesión y creen un token de usuario desde la configuración de la cuenta. El nombre y ubicación del menú pueden variar según la versión.

Guarden el token temporalmente en la sesión de PowerShell, sin escribirlo en el comando ni en un archivo del proyecto:

```powershell
$secureToken = Read-Host 'Token de SonarQube' -AsSecureString
$env:SONAR_TOKEN = [System.Net.NetworkCredential]::new('', $secureToken).Password
$env:SONAR_HOST_URL = 'http://localhost:9000'
```

No compartan el token ni lo peguen en capturas. Al terminar el análisis pueden quitarlo de la sesión:

```powershell
Remove-Item Env:SONAR_TOKEN
Remove-Item Env:SONAR_HOST_URL
```

### 7.4 Analizar el backend y el frontend

Ejecuten el análisis desde la raíz de `CodigoFuenteB`, donde están las carpetas `BACKEND` y `FRONTEND`:

```powershell
Set-Location 'S:\1- Sergio\Cursos\Seguridad Y Calidad en Desarrollo de Software\EP2\CodigoFuenteB'
sonar-scanner.bat "-Dsonar.projectKey=cuidarteplus" "-Dsonar.projectName=CuidartePlus" "-Dsonar.sources=BACKEND/src,FRONTEND/src" "-Dsonar.exclusions=**/node_modules/**,**/dist/**,**/coverage/**,**/uploads/**" "-Dsonar.sourceEncoding=UTF-8"
```

Si el ejecutable no está en el `PATH`, sustituyan `sonar-scanner.bat` por su ruta completa. No añadan el token como argumento `-D...` ni lo guarden en el archivo de configuración.

Cuando termine:

1. Abran el proyecto `cuidarteplus` en SonarQube.
2. Revisen los hallazgos por categoría y severidad: bugs, vulnerabilidades, hotspots de seguridad y code smells.
3. Revisen los hotspots manualmente: son puntos que requieren evaluación humana; no son automáticamente vulnerabilidades confirmadas.
4. Guarden la fecha, versión del scanner/servidor, alcance analizado y una exportación o capturas legibles de los resultados.
5. Registren cada hallazgo relevante en el informe. Añadan archivo/línea, impacto, evidencia, requisito asociado y recomendación; confirmen manualmente que el hallazgo aplica antes de llamarlo vulnerabilidad.

SonarQube no prueba que las funciones se comporten correctamente, no ejecuta por sí solo sus casos funcionales y no certifica cumplimiento legal. El análisis estático también puede producir falsos positivos y no detectar todos los defectos.

### 7.5 Ejecutar ESLint del frontend

El proyecto ya define ESLint en el frontend. Desde la raíz `CodigoFuenteB`:

```powershell
Set-Location 'S:\1- Sergio\Cursos\Seguridad Y Calidad en Desarrollo de Software\EP2\CodigoFuenteB\FRONTEND'
npm run lint
npm run build
```

Anoten el código de salida, errores y advertencias relevantes. `npm run build` verifica que se pueda construir el frontend; **no equivale a una prueba funcional ni a una auditoría de seguridad**.

El backend no muestra un script de pruebas automatizadas en su `package.json`. No afirmes que hay una suite automatizada si no existe o no se ejecutó. Documenten este límite y compleméntenlo con pruebas manuales de API/UI y revisión técnica.

## 8. Revisión manual y auditoría técnica

Con la ERS abierta y en el entorno local, revisen y documenten al menos:

- **Autenticación y sesión:** comportamiento de login/logout, manejo de errores y sesión.
- **Autorización:** diferencias entre paciente, médico y administrador; comprueben reglas del lado del servidor, no solo botones visibles en la interfaz.
- **Validación:** campos obligatorios, formato y límites esperados; observen si los errores son claros y no exponen información innecesaria.
- **Documentos:** tipos permitidos según la ERS, asociación al examen, acceso por usuario autorizado y comportamiento de carga/descarga.
- **Auditoría:** si las operaciones críticas previstas dejan un registro.
- **Configuración y datos:** almacenamiento de secretos, logs, HTTPS/TLS y copias de seguridad frente a lo especificado. Si algo no se puede comprobar localmente, indíquenlo como limitación.
- **Cumplimiento:** identifiquen qué obligaciones legales/normativas aplican con respaldo de una fuente oficial o del curso. No concluyan “cumple la ley” solo porque SonarQube no mostró alertas.

No ejecuten pruebas invasivas ni utilicen datos reales. Para cada observación anoten el requisito, método de revisión, evidencia y limitaciones.

## 9. Ejecutar los casos de prueba (testing dinámico)

Para cada caso diseñado:

1. Verifiquen que el entorno local está disponible y que se cumplen las precondiciones.
2. Usen cuentas y datos ficticios preparados para el caso.
3. Sigan los pasos exactamente y anoten el resultado real, sin modificarlo para que coincida con el esperado.
4. Comparen resultado esperado y real.
5. Asignen el estado: aprobado, fallido, bloqueado o no ejecutado.
6. Guarden evidencia legible y sin datos sensibles.
7. Si falla, creen un incidente con ID, caso relacionado, descripción, impacto, severidad, evidencia y recomendación.

Para NFR-PERF-1, midan la operación indicada por la ERS en condiciones anotadas (entorno, volumen y número de repeticiones) y comparen el resultado con el umbral de **menos de 300 ms**. Una medición aislada no demuestra el rendimiento bajo todas las cargas; no afirmen haber probado 200 usuarios concurrentes si no ejecutaron realmente esa prueba.

Para RF-4.4, la ERS indica un máximo de 10 segundos para carga/descarga. Registren el archivo ficticio y tamaño de prueba, las condiciones y el tiempo obtenido. No prueben documentos con información personal real.

## 10. Registrar y priorizar hallazgos

Usen la escala sugerida por la plantilla:

| Severidad | Orientación |
|---|---|
| Crítico | Bloquea una función esencial o el flujo principal. |
| Grave | Afecta una función importante, aunque el sistema siga funcionando parcialmente. |
| Moderado | Afecta escenarios específicos o funciones secundarias. |
| Leve | Defecto menor sin impacto funcional importante. |

No asignen una severidad solo por la etiqueta del scanner: consideren impacto, alcance, datos afectados y posibilidad de ocurrencia. Mantengan trazabilidad entre cada hallazgo, caso, requisito y evidencia.

## 11. Completar el informe de la plantilla

Usen la plantilla `Informe a ompletar.docx` como documento principal y agreguen apartados si hacen falta para cubrir la pauta.

1. **Portada:** nombre completo de integrantes, carrera, sección, docente y fecha.
2. **Introducción:** contexto de Cuidarte+, propósito del plan, alcance y estructura del informe.
3. **Ajustes de la evaluación anterior:** feedback recibido y cambio aplicado; si no hubo feedback disponible, declárenlo.
4. **Criterios de calidad:** criterios comprobables de calidad, seguridad, privacidad y cumplimiento que correspondan a la ERS.
5. **Plan de pruebas:** estrategia, etapas, tipos de pruebas y justificación, criterios de aceptación, herramientas, recursos humanos/técnicos, entorno, criterios de entrada/salida y prioridades.
6. **Matriz de cobertura:** todos los requisitos funcionales, no funcionales, reglas de negocio e interfaces que correspondan, asociados a uno o más casos y su estado.
7. **Diseño de casos:** incluir todos los campos solicitados por la plantilla, no solo el nombre y resultado.
8. **Ejecución:** resumen numérico coherente con el detalle; resultados por caso, fecha, ejecutor y evidencia.
9. **Incidentes:** hallazgos reales con severidad, impacto, evidencia y recomendación. Si no hubo hallazgos, aclaren que no se identificaron en el alcance ejecutado.
10. **Análisis ético y normativo:** impacto potencial de exponer datos médicos, minimización de datos, acceso por rol, límites de la evaluación y normas efectivamente revisadas.
11. **Conclusiones y limitaciones:** qué se comprobó, qué no se comprobó y acciones recomendadas.
12. **Anexos:** capturas/exportaciones de SonarQube, salida de ESLint/build y evidencias de pruebas, correctamente identificadas.

La tabla resumen debe cuadrar: total diseñado = ejecutado + bloqueado + no ejecutado; ejecutado = aprobado + fallido (salvo que el grupo defina estados adicionales y los explique).

## 12. Preparar la defensa y entregar

La defensa es individual: son 10 minutos de exposición y 5 minutos de preguntas. Cada integrante debe poder explicar:

1. Qué evalúa Cuidarte+ y por qué sus datos requieren protección.
2. Qué diferencia hay entre análisis estático, revisión manual y testing dinámico.
3. Qué encontró SonarQube y qué hallazgos verificaron manualmente.
4. Cómo relacionaron los casos de prueba con los requisitos de la ERS.
5. Qué pruebas se aprobaron, fallaron o no se pudieron ejecutar.
6. Qué limitaciones tuvo el trabajo y qué recomiendan hacer después.

Antes de entregar, apliquen el formato solicitado por la pauta: Word o PDF, títulos Arial 12, cuerpo Arial 11, interlineado 1,5, texto justificado, portada completa y evidencias legibles. Suban el informe y el material de apoyo al AVA.

## 13. Cierre del entorno

Cuando hayan terminado y guardado las evidencias, apaguen los servicios locales:

```powershell
Set-Location 'S:\1- Sergio\Cursos\Seguridad Y Calidad en Desarrollo de Software\EP2\CodigoFuenteB'
docker compose down
docker stop sonarqube-cuidarteplus
```

`docker compose down` detiene y elimina los contenedores del proyecto, pero normalmente conserva el volumen de base de datos. No usen opciones que borren volúmenes si no están seguros de que los datos de prueba pueden eliminarse. El contenedor de SonarQube queda detenido para conservar sus datos y puede iniciarse de nuevo con `docker start sonarqube-cuidarteplus`.

## Referencias

- [Instrucciones y pauta de evaluación](./Intrucciones%20y%20Pauta.md)
- [Especificación de requerimientos del software (ERS)](./Especificacion_de_requerimientos_del_Software.md)
- [Plantilla del informe](./Informe%20a%20ompletar.docx)
- [README del proyecto](./CodigoFuenteB/README.md)
- [Documentación oficial: instalación de SonarQube con Docker](https://docs.sonarsource.com/sonarqube-server/server-installation/from-docker-image/installation-overview/)
- [Documentación oficial: SonarScanner CLI](https://docs.sonarsource.com/sonarqube-server/analyzing-source-code/scanners/sonarscanner/)
- [Documentación oficial: análisis JavaScript/TypeScript/CSS](https://docs.sonarsource.com/sonarqube-server/analyzing-source-code/languages/javascript-typescript-css/)
