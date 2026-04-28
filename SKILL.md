---
name: git-commit-helper
description: >
  Genera mensajes de commit siguiendo el estándar Conventional Commits basándose
  en los cambios descritos por el usuario. Úsala siempre que el usuario mencione
  que hizo cambios en el código y quiera hacer un commit, cuando pregunte cómo
  redactar un mensaje de commit, cuando diga "qué commit pongo", "cómo hago el
  commit de esto", "ayúdame con el commit", o cuando describa archivos modificados,
  bugs corregidos, features agregadas o refactors realizados. También debe activarse
  cuando el usuario comparta output de `git diff`, `git status`, o liste archivos
  cambiados. No esperes a que el usuario diga "conventional commits" — actívala
  ante cualquier señal de que quiere commitear cambios.
---

# Git Commit Helper

Genera mensajes de commit profesionales siguiendo el estándar **Conventional Commits**.

## Formato del estándar

```
tipo(alcance opcional): descripción corta en imperativo

cuerpo opcional — explica el QUÉ y el POR QUÉ, no el cómo
```

### Tipos disponibles

| Tipo       | Cuándo usarlo                                              |
|------------|------------------------------------------------------------|
| `feat`     | Nueva funcionalidad                                        |
| `fix`      | Corrección de bug                                         |
| `chore`    | Configuración, dependencias, gitignore, scripts            |
| `refactor` | Reestructura código sin cambiar comportamiento             |
| `docs`     | Solo documentación o README                               |
| `test`     | Agregar o modificar pruebas                               |
| `style`    | Formato, espacios, comas — sin cambio de lógica           |
| `perf`     | Mejora de rendimiento                                      |
| `ci`       | Cambios en CI/CD, GitHub Actions, Railway, etc.           |

### Reglas de oro

1. **Imperativo, no pasado** — "agregar" no "agregué", "fix" no "fixed"
2. **Máximo 72 caracteres** en la primera línea
3. **Un commit = un cambio lógico** — no mezclar features con fixes
4. **Minúsculas** después del tipo
5. **Sin punto final**

---

## Cómo generar el mensaje

Cuando el usuario describa sus cambios:

1. **Identifica el tipo** — ¿Es algo nuevo? `feat`. ¿Arreglaste algo? `fix`. ¿Configuración? `chore`.
2. **Determina el alcance** (opcional) — el módulo o área afectada: `(correo)`, `(api)`, `(bd)`, `(excel)`.
3. **Redacta la descripción** — corta, en imperativo, en el mismo idioma que usa el usuario.
4. **Añade cuerpo** solo si el cambio no es obvio — explica el por qué, no el cómo.

### Formato de respuesta

Siempre presenta:
- El comando listo para copiar y pegar
- Una línea explicando por qué elegiste ese tipo
- Alternativa si hay ambigüedad

---

## Ejemplos

**Usuario:** "Agregué validación de correo en el formulario"
```bash
git commit -m "feat(api): agregar validación de formato de correo en /suscribir"
```
→ `feat` porque es comportamiento nuevo. Alcance `api` porque afecta el endpoint Flask.

---

**Usuario:** "Corregí que el nan% salía en el Excel en vez de N/A"
```bash
git commit -m "fix(excel): mostrar N/A en variaciones sin datos disponibles"
```
→ `fix` porque corrige un bug visual. Alcance `excel` porque solo afecta excel_builder.py.

---

**Usuario:** "Actualicé el .gitignore para ignorar los .db"
```bash
git commit -m "chore: ignorar archivos de base de datos SQLite en .gitignore"
```
→ `chore` porque es configuración pura, no afecta funcionalidad.

---

**Usuario:** "Moví las funciones de envío a un archivo separado y limpié el código"
```bash
git commit -m "refactor(correo): extraer lógica de envío a función _enviar_a_uno"
```
→ `refactor` porque reorganiza sin cambiar comportamiento externo.

---

**Usuario:** "Agregué gunicorn y flask al requirements y el Procfile para Railway"
```bash
git commit -m "chore: agregar configuración de deploy para Railway"

# Si quieres ser más específico:
git commit -m "chore: agregar Procfile, railway.json y dependencias de producción"
```
→ `chore` porque es infraestructura/configuración, no feature de usuario.

---

**Usuario:** "Cambié que en vez de leer el destinatario del .env ahora lo lee de la BD"
```bash
git commit -m "feat(correo): enviar reporte a suscriptores activos desde SQLite"
```
→ `feat` aunque modifica código existente — cambia el comportamiento de forma significativa.

---

## Casos con cuerpo del commit

Cuando el cambio necesita contexto:

```bash
git commit -m "fix(inegi): actualizar ID de indicador INPC a 910406" \
  -m "El ID anterior (334360) devolvía HTTP 400. El indicador correcto
es 'Inflación mensual interanual · Índice general' con ID 910406,
encontrado en el Constructor de Consultas del BIE de INEGI."
```

Usa cuerpo cuando:
- El cambio no es obvio con solo leer la primera línea
- Hubo una razón específica para el cambio (bug externo, cambio de API, decisión de diseño)
- Quieres dejar contexto para tu yo del futuro o para un equipo

---

## Commits múltiples

Si el usuario hizo varios cambios distintos, sugiere separarlos:

```bash
# En lugar de un commit mezclado:
git commit -m "feat: agregar BD y corregir Excel y actualizar readme"

# Sugiere esto:
git add modules/suscriptores.py
git commit -m "feat: agregar módulo de suscriptores con SQLite"

git add modules/excel_builder.py
git commit -m "fix(excel): mostrar N/A en variaciones sin datos disponibles"

git add README.md
git commit -m "docs: actualizar README con instrucciones de suscripción"
```
