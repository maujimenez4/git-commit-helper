# git-commit-helper

Una skill para Claude que genera mensajes de commit profesionales siguiendo el estándar **Conventional Commits**, lista para copiar y pegar.

## ¿Qué hace?

Describe tus cambios en lenguaje natural y Claude te devuelve el comando `git commit` correcto, con el tipo adecuado, el alcance y la descripción en imperativo.

```
Tú:    "Corregí que el nan% salía en el Excel en vez de N/A"

Claude: git commit -m "fix(excel): mostrar N/A en variaciones sin datos disponibles"
        → fix porque corrige un bug visual
```

No necesitas saber nada del estándar — Claude lo aplica por ti.

---

## Instalación

1. Descarga el archivo [`git-commit-helper.skill`](./git-commit-helper.skill)
2. Ve a **Claude.ai → Settings → Skills**
3. Sube el archivo

Listo. A partir de ahí Claude la activa automáticamente cuando mencionas que hiciste cambios en el código.

---

## Cómo usarla

Simplemente describe lo que hiciste. No hace falta ningún comando especial:

```
"Agregué validación de correo en el formulario"
"Hice estos cambios, qué commit pongo"
"Corregí el bug del 400 en la API de INEGI"
"Refactoricé el módulo de envío"
```

También puedes pegar el output de `git status` o `git diff` y Claude genera el commit basándose en los archivos modificados.

---

## El estándar Conventional Commits

El formato es:

```
tipo(alcance): descripción en imperativo
```

| Tipo | Cuándo usarlo |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `chore` | Configuración, dependencias, gitignore |
| `refactor` | Reorganizar código sin cambiar comportamiento |
| `docs` | Solo documentación o README |
| `test` | Agregar o modificar pruebas |
| `style` | Formato, espacios — sin cambio de lógica |
| `perf` | Mejora de rendimiento |
| `ci` | CI/CD, GitHub Actions, Railway, etc. |

### Reglas de oro

- **Imperativo** — "agregar" no "agregué", "fix" no "fixed"
- **Máximo 72 caracteres** en la primera línea
- **Un commit = un cambio lógico**
- **Sin punto final**

### Ejemplos reales

```bash
git commit -m "feat(api): agregar endpoint POST /suscribir con validación"
git commit -m "fix(excel): mostrar N/A en variaciones sin datos disponibles"
git commit -m "chore: agregar configuración de deploy para Railway"
git commit -m "refactor(correo): extraer lógica de envío a función privada"
git commit -m "docs: agregar URL de producción en README"
git commit -m "ci: agregar workflow de GitHub Actions para deploy automático"
```

### Con cuerpo (para cambios que necesitan contexto)

```bash
git commit -m "fix(inegi): actualizar ID de indicador INPC a 910406" \
  -m "El ID anterior devolvía HTTP 400. El correcto se encontró
en el Constructor de Consultas del BIE de INEGI."
```

---

## ¿Por qué Conventional Commits?

- Tu historial de Git se vuelve **legible** para cualquiera que revise tu repo
- Facilita generar **changelogs automáticos**
- Es el estándar que usan equipos profesionales — verlo en un portafolio comunica que sabes trabajar en equipo
- Herramientas como **semantic-release** lo usan para versionar automáticamente

---

## Estructura del repo

```
git-commit-helper/
├── SKILL.md                  ← Instrucciones para Claude
├── README.md                 ← Este archivo
└── git-commit-helper.skill   ← Archivo de instalación
```

---

## Recursos

- [Conventional Commits — especificación oficial](https://www.conventionalcommits.org/es/v1.0.0/)
- [Angular commit guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit) — de donde nació el estándar
