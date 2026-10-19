---
title: "Lección 12 - Publica el sitio con GitHub Pages"
description: "Utiliza la aplicación GitHub Copilot para configurar un workflow de Pages, revisar la pull request y comprobar que Space Quiz está publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que tenga una dirección web pública. Copilot puede preparar el workflow y ayudarte con la publicación, pero tú debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una implementación en GitHub Pages;
- revisarás el workflow y la pull request antes de combinarla;
- seguirás la implementación y comprobarás el sitio publicado.

## Situación

Space Quiz ya está en un repositorio de GitHub, pero todavía no hay una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él mediante una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía esta solicitud:

```plaintext
Habilita GitHub Pages para este repositorio. Crea un workflow de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages. Añade un README que describa la aplicación y cómo ejecutarla localmente, si aún no existe. Crea una pull request con estos cambios y combínala cuando se superen las comprobaciones obligatorias. Supervisa la implementación hasta que se complete correctamente y, después, indica el localizador uniforme de recursos (URL) del sitio web publicado. Si no tienes permiso para habilitar Pages o combinar la pull request, dime exactamente qué debo aprobar o hacer; no indiques que se ha completado hasta que el sitio esté disponible.
```

![Diagrama de tres etapas: una pull request de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Esta solicitud pide al agente que cree y combine una pull request y cambie la configuración de Pages del repositorio. Revisa el diff, los permisos de implementación y las solicitudes de aprobación de GitHub antes de continuar.

## Revisa el workflow y comprueba la implementación

1. Comprueba que el README solo se haya añadido si el repositorio aún no tenía uno.
2. Revisa los cambios de la pull request, incluidos los pasos de compilación e implementación del workflow.
3. Combina la pull request solo cuando se superen las comprobaciones obligatorias.
4. Sigue la ejecución de implementación de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y comprueba que se carga Space Quiz.

Si el agente no puede habilitar Pages o combinar la pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que se complete la implementación y se cargue el sitio publicado.

## Resumen y siguientes pasos

Space Quiz ya está disponible en un sitio publicado y has comprobado la implementación en lugar de confiar únicamente en el informe del agente. Continúa con la [lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
