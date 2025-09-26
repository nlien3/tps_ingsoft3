# 📚 Resoluciones de TPs

Este repositorio agrupa las entregas de los TPs de la materia, pero **algunos proyectos se mantienen en repositorios separados**.

## 🔹 TP1

- Carpeta local: `tp-1/2025_TP01_RepoBase`
- Repositorio: [2025_TP01_RepoBase](https://github.com/nlien3/2025_TP01_RepoBase)

## 🔹 TP2

- Carpeta local: `tp-2/notas-app`
- Repositorio: [notas-app](https://github.com/nlien3/notas-app)

Este TP introduce el proyecto **NotasApp**, que luego se reutiliza y expande en los TPs siguientes (TP3 y TP4).

### 🐳 Docker

Dentro de `tp-2/` se agregó una carpeta `docker/` con un archivo `docker-compose.yml`.

Para levantar los servicios:

```bash
cd tp-2/docker
docker compose pull
docker compose up -d
```

## 🔹 TP3

- Carpeta local: `tp-3/azure-notas-app`
- **Repositorio remoto**: [NotasApp en Azure DevOps (Azure Repos)](https://nicoingsoft3@dev.azure.com/nicoingsoft3/NotasApp/_git/NotasApp)  
  _(⚠️ A diferencia de TP2, este proyecto ya no está en GitHub sino en Azure DevOps)._

Continuamos trabajando sobre **NotasApp**, ahora integrando el código en **Azure Repos** y gestionando el trabajo en **Azure Boards**.

### 📂 Archivos adicionales en este repo

Dentro de la carpeta `tp-3/` también se incluyó:

- `decisiones.md` → documento con las decisiones de diseño y configuración del TP3.

## 🔹 TP4

- Carpeta local: `tp-4/`
- (contenido incluido directamente en este repo)

### 📂 Archivos dentro de esta carpeta

- `azure-pipelines.yml` → definición del pipeline de CI en YAML.
- `decisiones.md` → documento con las decisiones de diseño y configuración del TP4.
