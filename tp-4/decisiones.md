# Decisiones del Proyecto – TP4 Azure DevOps Pipelines

## 1. Stack elegido y estructura del repo

Elegimos trabajar con **Next.js 15 (App Router)** porque nos permite tener en un mismo proyecto tanto el **frontend** (componentes, páginas) como el **backend** básico mediante **API Routes**.  

Si bien la consigna sugería separar en `/front` y `/back`, en nuestro caso el framework unifica ambas capas. Para cumplir igualmente con el espíritu del TP, decidimos **separar la ejecución en dos jobs dentro del pipeline**:
- **Job Build Next.js (UI)** → instalación de dependencias, lint y build de la aplicación.
- **Job Verificaciones API** → instalación, lint y tests sobre los route handlers de la API.

La estructura de nuestro repo es la típica de un proyecto Next.js con App Router:

azure-notas-app/
├─ app/
│   ├─ api/        # Backend (API Routes)
│   ├─ page.tsx    # Frontend principal
│   └─ layout.tsx  # Layout de la aplicación
├─ components/     # Componentes de UI
├─ public/         # Archivos estáticos
├─ sql/            # Scripts SQL
├─ package.json
├─ azure-pipelines.yml
└─ …

---

## 2. Diseño del pipeline

Definimos un pipeline en YAML bajo el principio **Pipelines as Code** para que quede versionado junto al código.  

Características principales:
- **Trigger en rama main** → cada push dispara CI automáticamente.
- **Pool Self-Hosted** → usamos un agente instalado en nuestra propia máquina (Mac ARM).
- **Variables** → definimos la versión de Node como `20.x` para garantizar consistencia.
- **Jobs separados**:
  - `Build Next.js`: `npm ci`, `npm run build`, empaquetado y publicación del artefacto.
  - `Verificaciones API`: `npm ci`, `npm run lint`, `npm run test`.

El artefacto publicado se llama **`next-app`** e incluye:
- La carpeta `.next` (resultado de build).
- La carpeta `public` (archivos estáticos).
- Archivos de configuración mínimos (`package.json`, `package-lock.json`, `next.config.*`).

---

## 3. Agente Self-Hosted

Creamos un **pool llamado `SelfHosted-NotasApp`** y registramos un agente local (`MacAgent1`).  

Inicialmente instalamos el agente **x64** y detectamos un warning de emulación en ARM: 

"X64 emulation is known to cause hangs in the Agent process. Please use the native (ARM) Agent."

Para solucionarlo, removimos el agente x64 y configuramos el **agente nativo ARM**, lo cual eliminó los warnings. Finalmente lo instalamos como **servicio**, de modo que queda disponible siempre que encendemos la máquina.

---

## 4. Evidencias

1. **Agente Self-Hosted Online**  
   Captura del agente `MacAgent1` en el pool `SelfHosted-NotasApp`, en estado verde (online).

2. **Pipeline YAML versionado**  
   Archivo `azure-pipelines.yml` en la raíz del repo.

3. **Ejecución de CI exitosa**  
   - Job `Build Next.js`: logs mostrando `npm run build` completado y artefacto publicado.  
   - Job `Verificaciones API`: logs mostrando `npm run test` (en nuestro caso sin tests definidos, pero el paso se ejecuta).  

4. **Artifact publicado**  
   Captura de la pestaña *Artifacts* mostrando `next-app` (~71 MB).

---

## 5. Justificación de decisiones

- **YAML vs Classic** → elegimos YAML porque nos permite versionar la definición del pipeline junto al código, lo cual garantiza trazabilidad, repetibilidad y buenas prácticas modernas de CI/CD.
- **Agente Self-Hosted vs Microsoft-Hosted** → elegimos Self-Hosted porque necesitamos:
  - Controlar la versión de Node.js (20.x en ARM).  
  - Acceso a nuestro entorno local (por ejemplo, DB, Docker o puertos si hiciéramos despliegues).  
  - Evitar limitaciones de tiempo de los agentes Microsoft-Hosted.
- **Separación de jobs (UI/API)** → aunque Next.js es unificado, dividimos la CI en dos jobs distintos para reflejar la separación conceptual de frontend y backend.

---

## 6. Próximos pasos (posibles mejoras)

- Implementar un segundo stage de **CD** que levante automáticamente la app en un entorno (Docker o Azure Web Apps).  
- Agregar un set de **tests unitarios** para la API y de **tests de integración** para el frontend.  
- Extender el pipeline con validación de calidad de código (ej: SonarQube).

---

## 7. Conclusión

Logramos configurar un pipeline en YAML que:
- Corre en un agente Self-Hosted.  
- Construye y valida nuestra aplicación Next.js (UI + API).  
- Publica un artifact reutilizable (`next-app`).  
