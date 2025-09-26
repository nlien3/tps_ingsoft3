# Decisiones del Proyecto - NotasApp

## 1. Configuración inicial del proyecto

### Organización y proyecto
Creamos una organización en **Azure DevOps** para centralizar el trabajo en equipo y facilitar la gestión del ciclo de vida del proyecto. Dentro de esta organización, configuramos un proyecto llamado **NotasApp**, que será la base para el desarrollo de nuestra aplicación.

### Metodología elegida
Decidimos utilizar el **proceso Agile** de Azure DevOps. La elección se fundamenta en los siguientes puntos:

- **Simplicidad**: Agile ofrece una estructura clara con Epic → User Story → Task, que se adapta muy bien al tamaño y alcance de nuestro proyecto.
- **Iteraciones cortas**: Nos permite organizar el trabajo en sprints y entregar valor en ciclos cortos.
- **Escalabilidad**: Aunque el equipo actualmente es de 2 personas, Agile nos permite crecer sin problemas en caso de que se incorporen más integrantes.
- **Visibilidad**: Las herramientas de Azure Boards bajo Agile son fáciles de usar y nos dan buena trazabilidad del trabajo.

### Equipos y áreas
Creamos un único equipo de trabajo: **NotasApp Team**.  
Dentro de este equipo configuramos un único **Area Path** llamado `NotasApp`, que agrupa todos los work items, y definimos **Iteration Paths** para los distintos sprints del proyecto.

---

## 2. Gestión del trabajo con Azure Boards

### Estructura de trabajo definida
Adoptamos la siguiente jerarquía de work items en Azure Boards:

- **Epic**  
  - `Gestión de notas del usuario`

- **User Stories** (todas vinculadas al Epic)  
  1. Como usuario quiero **crear una nota** para guardar ideas.  
     - Task: Diseñar formulario (UI, validaciones)  
     - Task: API POST `/api/notes` y guardar en MySQL  
     - Bug: Se guarda nota sin título (debería fallar)  
  2. Como usuario quiero **editar una nota** para mantenerla actualizada.  
     - Task: UI de edición  
     - Task: API PUT `/api/notes/:id` + actualizar en DB  
  3. Como usuario quiero **marcar una nota como completada** para organizarme mejor.  
     - Task: Checkbox y actualización de estado  
     - Task: Refrescar lista tras cambiar done  
     - Bug: El estado “completada” no persiste al recargar  

En total:  
- **1 Epic**  
- **3 User Stories**  
- **6 Tasks** (2 por cada User Story)  
- **2 Bugs** de ejemplo  

### Sprint configurado
Creamos un **Sprint de 2 semanas**:  
- Nombre: **Iteration 1**  
- Fechas: **25/09/2025 – 09/10/2025**  
- Duración: 11 días hábiles  

Todos los work items (Epic, User Stories, Tasks y Bugs) fueron asignados al **Iteration Path: NotasApp\Iteration 1**. Esto asegura que todo el alcance definido quede planificado dentro del Sprint 1.

---

## Resumen de la estructura

- **Epic**: 1  
- **User Stories**: 3  
- **Tasks**: 6  
- **Bugs**: 2  
- **Sprint activo**: Iteration 1 (2 semanas)

Esta estructura nos permitirá gestionar de manera ordenada el desarrollo de la aplicación de notas, asegurando trazabilidad desde las funcionalidades de alto nivel (Epic) hasta las tareas técnicas de implementación y validación (Tasks y Bugs).

---

## 3. Control de versiones con Azure Repos

### Repositorio del proyecto
Importamos el código base desde GitHub ([notas-app](https://github.com/nlien3/notas-app)) hacia un repositorio en **Azure Repos**, dentro del proyecto **NotasApp**.  
Esto nos permite centralizar el control de versiones junto con las demás herramientas de Azure DevOps (Boards, Pipelines, etc.).

### Políticas de la rama principal
Para asegurar la calidad del código en la rama `main`, configuramos las siguientes **branch policies**:

- **Pull Request obligatorio**: ningún cambio puede integrarse directamente en `main`.
- **Mínimo 1 reviewer**: se requiere al menos una aprobación antes de completar el merge.  
- **Merge mediante PR**: los cambios se integran solo a través de revisiones controladas.

Esto garantiza que cada modificación sea revisada, aprobada y trazable.

### Estrategia de ramas
Adoptamos una convención de ramas basada en prefijos `feature/`:

- `feature/crear-nota` → para implementar la funcionalidad de creación de notas.  
- `feature/editar-nota` → para la funcionalidad de edición de notas.  

La rama `main` queda protegida y estable, mientras que el desarrollo ocurre en ramas de feature que luego se integran mediante PR.

### Cambios realizados
En cada rama de feature realizamos pequeños cambios controlados (por ejemplo, `console.log` de prueba en funcionalidades específicas) para poder comprobar el flujo completo de versionado y revisiones.

### Pull Requests
Creamos y completamos al menos **2 Pull Requests**:

1. **PR de `feature/crear-nota` → main**  
   - Incluyó un cambio de prueba en la creación de notas.  
   - Fue aprobado y mergeado con éxito.  

2. **PR de `feature/editar-nota` → main**  
   - Incluyó un cambio de prueba en la edición de notas.  
   - Resolví un conflicto en `page.tsx`, y luego fue aprobado y mergeado.  

Cada PR fue vinculado a su respectiva **User Story** en Azure Boards, garantizando trazabilidad entre el código y los requerimientos.

### Sincronización local
Tras completar los merges, actualizamos la rama `main` local con:

```bash
git switch main
git pull origin main