---
title: "Lección 8 - Crea y combina la PR de la función"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste manualmente la PR de la valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, la habilidad y el agente de control de calidad en varias PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás todos los cambios de la función y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Situación

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control sobre la autorización de la combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR —corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a basar la rama cuando es necesario— y, después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request** personalmente. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Ya has creado todo el código. Revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge se ocupe del resto.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni la falta de permisos. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar, probar y validar código, y ahora gestionar las solicitudes de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionaste todos los cambios de la función y las pruebas de verificación.
- creaste la PR de filtrado.
- activaste Agent Merge solo después de la revisión y confirmaste que se combinó la PR.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestiona solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
