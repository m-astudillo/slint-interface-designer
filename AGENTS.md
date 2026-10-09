## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)

## Reglas Generales

- La arquitectura del proyecto es Scream Architecture. Para más información revisa ./docs/architecture.md
- Cuando el usuario haga un propose de openspec utiliza la skill grill-me.
- Cada vez que se ejecute el comando openspec-apply, se debe ejecutar el manual de react doctor ubicado en ./docs/react-doctor.md (flujo de ejecución, análisis y corrección de errores).
- El gestor de paquetes es **pnpm**. Siempre se debe usar `pnpm` para todas las operaciones de gestión de dependencias. Para más información revisa ./docs/pnpm.md
- Siempre que agregues, actualizes o elimines una dependencia actualiza el tech stack ubicado en ./docs/stack.md
- Las specs generadas deben estar en español a menos que el usuario te lo indique antes, el código y su documentación deben estar en inglés
- Para revisar la estructura del proyecto usa el comando ls -T --no-permissions --no-filesize --no-user --no-time --no-git --git-ignore
- Antes de empezar a escribir código revisa las skills instaladas.
- Siempre que se necesite documentación de una librería, framework o herramienta, se debe usar **Context7** para obtenerla antes de escribir código. Esto aplica a:
  - Decisiones de API o funcionalidad de librerías.
  - Resolver dudas sobre configuración o patrones de uso.
  - Verificar comportamientos antes de asumirlos.
- Cuando no encuentres documentación usando **Context7** debes pedirle al usuario la documentación de la dependencia y respaldarla en ./docs/dependencies/[component].md
- **Nunca debes instalar una dependencia sin preguntar primero al usuario.** Antes de agregar cualquier paquete se debe:
  1. Informar qué dependencia se va a instalar en la conversación y en la spec; y el por qué se desea instalar la dependencia.
  2. Esperar la confirmación del usuario antes de ejecutar el comando de instalación.
- Antes de pensar en instalar una nueva dependencia, revisa las dependencias instaladas y utiliza esa dependencia.
- No asumir: si algo no está claro, preguntar.
- No cometer código sin instrucción explícita del usuario.
- Mantener los cambios mínimos y enfocados.
- Respetar las convenciones y patrones existentes del proyecto.
- Genera subagentes cuando lo consideres necesario.
- Las propuestas de spec futuras se encuentran en ./docs/specs/proposal/[spec-name].md y las spec pendientes a implementar se encuentran en ./docs/specs/pending/[spec-name].md. Una vez desarrolladas estas se deben borrar de los respectivos directorios.

---

## Conventional Commits

Todos los commits deben seguir la especificación de [Conventional Commits](https://www.conventionalcommits.org/). El formato del mensaje es:

```
<tipo>[<scope opcional>]: <descripción>

[cuerpo opcional]
```

**Tipos permitidos:**

- `feat`: Nueva funcionalidad.
- `fix`: Corrección de un bug.
- `docs`: Cambios en documentación.
- `style`: Cambios de formato sin afectar código (espacios, puntuación, etc.).
- `refactor`: Cambio de código que no corrige un bug ni agrega funcionalidad.
- `test`: Agregar o corregir tests.
- `chore`: Cambios en build process o herramientas auxiliares.

Ejemplo: `feat(auth): add login endpoint`

---
