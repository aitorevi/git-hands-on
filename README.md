# 🚀 Git para Humanos: De Cero a Pull Request

Bienvenido/a al taller de Git de LeanMind. Si estás aquí, probablemente acabas de escuchar palabras como "commit", "rama" o "conflicto" y no sabes muy bien qué significan. No te preocupes, ese es exactamente el punto de partida correcto.

---

## ¿Qué es Git? Dos metáforas para entenderlo

### 🕰️ La Máquina del Tiempo
Git es como una máquina del tiempo para tu código. Cada vez que haces un **commit**, estás tomando una foto del estado de tu proyecto en ese momento. Si en el futuro algo sale mal, puedes volver a cualquier foto anterior. Sin Git, si borras algo importante, se fue para siempre. Con Git, siempre puedes recuperarlo.

### 🧱 El Juego de Construcción en Equipo
Imagina que tú y tres compañeros/as estáis construyendo con LEGO, pero cada uno en su propia mesa de trabajo. Git es el sistema que permite que cada uno construya su pieza por separado y luego las **combine** en la construcción final sin que nadie pise el trabajo del otro. A esa mesa de trabajo separada la llamamos **rama**.

---

## 📚 Tabla de Contenidos

| Ejercicio | Tema | Tiempo estimado |
|-----------|------|----------------|
| [01 - Mi Primer Commit](./01-mi-primer-commit/README.md) | Staging Area y commits con sentido | 20 min |
| [02 - Ramas y Universos](./02-ramas-y-universos/README.md) | Crear y navegar entre ramas | 20 min |
| [03 - Trabajo en Equipo](./03-trabajo-en-equipo/README.md) | Push, Pull y Pull Requests | 25 min |
| [04 - El Temido Conflicto](./04-el-temido-conflicto/README.md) | Provocar y resolver conflictos | 30 min |

---

## 🆘 Comandos de Supervivencia

Si alguna vez estás perdido/a, esta tabla es tu ancla.

| Comando | ¿Qué hace? |
|---------|------------|
| `git status` | "¿Qué está pasando ahora mismo?" |
| `git add <archivo>` | Prepara un archivo para el commit |
| `git add .` | Prepara **todos** los cambios para el commit |
| `git commit -m "mensaje"` | Guarda la foto del estado actual |
| `git log --oneline` | Ver el historial de commits en resumen |
| `git branch` | Ver en qué rama estás |
| `git switch -c <nombre>` | Crear una nueva rama y saltar a ella |
| `git switch <nombre>` | Saltar a una rama que ya existe |
| `git pull` | Traer los cambios del repositorio remoto |
| `git push` | Enviar tus cambios al repositorio remoto |

> 💡 **Regla de oro:** ante la duda, ejecuta `git status`. Siempre. Te dirá exactamente dónde estás y qué puedes hacer a continuación.

---

## ⚙️ Antes de empezar

Asegúrate de tener configurado tu nombre y email en Git. Estos datos aparecerán en cada commit que hagas:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

¡Ya estás listo/a! Empieza por el ejercicio 01. 👇
