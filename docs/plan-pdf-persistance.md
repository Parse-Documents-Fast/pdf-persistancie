# plan.md — pdf-persistance

## Qué hay que construir
Persistencia del contenido en Markdown (nunca binarios) en MongoDB, más la lógica de detección de duplicados que ya existía en el monolito, adaptada a este esquema más simple.

## Cómo construirlo, en orden
1. Definir el modelo Mongo: `content` (Markdown), `checksum`, `original_format` (metadata, no afecta cómo se almacena), `title`, `created_at`.
2. CRUD básico: crear, listar, obtener por ID, eliminar — misma forma que ya existe en el monolito (`get_pdf_or_raise`, etc.), adaptado al nuevo modelo.
3. Búsqueda por checksum para detección de duplicados, igual que en el monolito.
4. Exponer estas operaciones para que `pdf-main` las consuma — decidir si es vía HTTP interno o llamada directa, según lo que se resuelva en el `/spec` de `pdf-main`.

## Ya resuelto
La lógica de negocio (qué significa "no encontrado", cómo se maneja un duplicado) ya está probada en el monolito — este repo es principalmente portar esa lógica a un modelo más simple (un solo campo de contenido en vez de PDF+texto extraído por separado).

## Actualización — formato canónico (ADR-0005)
El contenido persistido pasa de HTML a Markdown directamente — ya no hay conversión de por medio en la ingesta de un Markdown subido (passthrough), ni en la descarga en formato Markdown (ya está en ese formato). Solo la descarga en PDF necesita pasar por `pdf-converter`.
