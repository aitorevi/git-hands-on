# 🤝 03 - Trabajo en Equipo: Remoto y Pull Requests

## Teoría ELI5: Local vs Remoto

Hasta ahora todo lo que has hecho vive en **tu ordenador**. Git llama a esto el repositorio **local**. Cuando trabajas en equipo, necesitas un sitio compartido donde todo el mundo pueda sincronizar su trabajo: eso es el repositorio **remoto** (GitHub, GitLab, etc.).

La relación es simple:

- **`git push`** → "Envío mis commits locales al servidor compartido"
- **`git pull`** → "Traigo al repositorio local los commits que han subido mis compañeros/as"

```
Tu ordenador (local)          GitHub (remoto)
┌─────────────────┐           ┌──────────────┐
│  Tu rama local  │  ──push→  │  Rama remota │
│                 │  ←pull──  │              │
└─────────────────┘           └──────────────┘
```

Piensa en el remoto como un **Dropbox inteligente para código**, pero que además guarda el historial completo y controla quién puede fusionar qué.

---

## ¿Qué es un Pull Request?

Un **Pull Request** (PR) o **Merge Request** (en GitLab) es una **solicitud formal** para que tu rama sea fusionada en `main`. No es un comando de Git, es una funcionalidad de GitHub.

Un PR tiene tres funciones clave:
1. **Revisión de código** (Code Review): tus compañeros/as leen tu código y sugieren mejoras antes de que entre a `main`.
2. **Discusión**: se pueden dejar comentarios línea a línea.
3. **Aprobación**: normalmente se necesita al menos una aprobación de otro miembro del equipo.

En el mundo profesional, **todo el código pasa por un PR**. Es la principal garantía de calidad de un equipo.

---

## 🎯 El Reto

Para este ejercicio necesitas tener el repositorio clonado desde GitHub y tu rama del ejercicio 02.

**1. Asegúrate de estar en tu rama de feature:**
```bash
git switch feature/[tu-nombre]-saludo
```

**2. Sube tu rama al repositorio remoto por primera vez:**
```bash
git push -u origin feature/[tu-nombre]-saludo
```
La flag `-u` conecta tu rama local con la remota. A partir de ahora, en esa rama, bastará con `git push`.

**3. Ve a GitHub en el navegador.** Verás un banner amarillo que dice algo como "Compare & pull request". Haz clic en él.

**4. Rellena el formulario del PR:**
- **Título:** `feat: añadir saludo de [tu nombre]`
- **Descripción:** Explica brevemente qué has hecho y por qué. Una o dos frases bastan.

**5. Asigna a un compañero/a como revisor** (sección "Reviewers" a la derecha).

**6. Crea el Pull Request** y espera a que tu compañero/a lo revise.

**7. Mientras esperas, revisa tú el PR de otro compañero/a.** Deja al menos un comentario constructivo.

---

## 💡 Pro Tip: Mantén tu rama actualizada con `main`

Mientras trabajas en tu rama, `main` puede haber avanzado (otros compañeros/as han fusionado sus PRs). Para no quedarte atrás:

```bash
git switch main
git pull
git switch feature/[tu-nombre]-saludo
git merge main
```

Hacer esto frecuentemente (al menos una vez al día) reduce mucho la probabilidad de conflictos grandes. Los conflictos pequeños y frecuentes son mucho más fáciles de resolver que uno grande al final.

---

## ✅ ¿Cómo sé que lo he hecho bien?

Tu PR debe aparecer en la pestaña "Pull requests" del repositorio en GitHub con estado "Open". Debes haber recibido o enviado al menos una revisión. El flujo completo es: rama local → push → PR → review → merge.
