---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge y confirma que la PR se ha combinado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de aseguramiento de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una misma sesión y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación;
- autorizarás Agent Merge únicamente después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo pendiente de la PR, pero mantener el control del desarrollador sobre la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo pendiente para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la solicitud y resuelve los obstáculos que impiden combinarla: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y reorganiza la base cuando es necesario. La combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esta tarea, pero su capacidad para editar código y combinar también requiere tu autorización explícita. Revisa las acciones que puede realizar y el trabajo antes de concederle permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora dejaremos que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge comienza. Se encargará de lo siguiente:

- crear la solicitud de incorporación de cambios con un título y una descripción;
- si has iniciado la sesión desde una incidencia, incluir una referencia a esa incidencia en el cuerpo de la descripción;
- reorganizar la base o resolver posibles conflictos con la rama de destino;
- supervisar el proceso de CI para asegurar que se superen todas las comprobaciones;
- supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizar cambios para resolverlos;
- combinar automáticamente la PR cuando todo haya finalizado correctamente, si así se lo indicas.

Pidamos a Agent Merge que combine la PR cuando se supere todo.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Combina la PR cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No eludas las protecciones del repositorio.
    ```

9. Sigue el proceso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR sea **Merged**. Tener habilitado el permiso para combinar o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el flujo de las solicitudes de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- has inspeccionado la PR completa de la funcionalidad y las pruebas de verificación;
- has autorizado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
