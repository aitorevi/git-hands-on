# 🔍 07 - Diff y Log: El Arte de Inspeccionar

## Teoría ELI5

Si `git commit` es como tomar una foto, `git diff` es como comparar dos fotos para ver exactamente qué ha cambiado. Y `git log` es el álbum de fotos completo con todos los metadatos.

Aprender a leer bien el output de estos dos comandos te convierte en alguien que entiende el historial de un proyecto en lugar de simplemente contribuir a él a ciegas.

---

## `git diff`: Comparar cambios

### Las tres situaciones más comunes

```
Working Directory   →   Staging Area   →   Repositorio (commits)
      │                     │                     │
      └── git diff ──────────┘                     │
      │                                            │
      └── git diff HEAD ───────────────────────────┘
                           │                       │
                           └── git diff --cached ──┘
```

| Comando | ¿Qué compara? |
|---------|--------------|
| `git diff` | Cambios en tu Working Directory que **no están en Staging** |
| `git diff --cached` | Cambios que **sí están en Staging** (los que harán el próximo commit) |
| `git diff HEAD` | **Todos** los cambios locales respecto al último commit |
| `git diff main..feature/x` | Diferencias entre dos ramas |
| `git diff abc1234..def5678` | Diferencias entre dos commits concretos |

### Cómo leer el output de `diff`

```diff
diff --git a/archivo.txt b/archivo.txt
index 83db48f..f735c0b 100644
--- a/archivo.txt       ← versión antigua
+++ b/archivo.txt       ← versión nueva
@@ -1,4 +1,5 @@        ← posición en el archivo
 línea sin cambios
-línea que se ha eliminado     ← en rojo
+línea que se ha añadido       ← en verde
 otra línea sin cambios
```

---

## `git log`: Explorar el historial

### De básico a avanzado

```bash
# Lo más básico
git log

# Compacto: una línea por commit
git log --oneline

# Con gráfico de ramas (muy útil en proyectos con varias ramas)
git log --oneline --graph --all

# Ver quién cambió qué y cuándo (últimos 10 commits)
git log --oneline -10

# Buscar commits por mensaje
git log --grep="login"

# Ver commits de un autor concreto
git log --author="Ana"

# Ver qué archivos cambió cada commit
git log --stat

# Ver el diff completo de cada commit
git log -p
```

### `git blame`: quién escribió cada línea

```bash
git blame archivo.txt
```

Muestra cada línea del archivo con el hash del commit, el autor y la fecha en que se escribió. Útil para entender el contexto de un código sin tener que preguntar.

> 🤫 **Nota cultural:** a pesar del nombre, `git blame` no se usa para culpar a nadie. Se usa para entender el contexto y poder preguntar a la persona correcta.

---

## 🎯 El Reto

**1. Crea una situación con cambios en distintos estados:**

```bash
# Modifica un archivo y añádelo al staging
echo "cambio preparado" >> README.md
git add README.md

# Modifica otro archivo sin añadirlo al staging
echo "cambio sin preparar" >> 01-mi-primer-commit/README.md
```

**2. Ahora inspecciona los tres estados de diff:**

```bash
# Solo los cambios fuera del staging
git diff

# Solo los cambios dentro del staging
git diff --cached

# Todos los cambios juntos
git diff HEAD
```

Fíjate en que cada comando muestra cosas distintas.

**3. Practica con git log:**

```bash
# Ver el historial compacto
git log --oneline

# Ver el historial con el gráfico de ramas
git log --oneline --graph --all

# Ver los archivos que cambió cada commit
git log --stat --oneline
```

**4. Usa git blame en un archivo:**

```bash
git blame README.md
```

**5. Limpia los cambios de prueba antes de continuar:**

```bash
git restore README.md
git restore 01-mi-primer-commit/README.md
```

---

## 💡 Pro Tip: El alias que todo el mundo acaba creando

El comando `git log --oneline --graph --all --decorate` es muy largo para escribirlo siempre. Puedes crear un alias:

```bash
git config --global alias.lg "log --oneline --graph --all --decorate"
```

A partir de ese momento, `git lg` hace todo eso. Los aliases de Git viven en `~/.gitconfig` y se sincronizan si usas dotfiles.

---

## ✅ ¿Cómo sé que lo he hecho bien?

Al final del reto, `git status` debe mostrar "nothing to commit, working tree clean". Habrás visto la diferencia entre los tres modos de `git diff` y sabrás leer el output con las líneas en rojo y verde.
