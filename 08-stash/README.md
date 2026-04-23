# 🗄️ 08 - Stash: El Cajón de los Cambios Pendientes

## Teoría ELI5: ¿Para qué sirve el stash?

Imagina que estás en medio de implementar una funcionalidad, con cinco archivos a medias, y de repente tu compañero/a te dice: "¡Oye, hay un bug urgente en producción, necesito que lo mires ahora!". No puedes hacer commit de lo que tienes porque está incompleto. Y si cambias de rama, Git puede quejarse o mezclar tus cambios.

**`git stash`** es como abrir un cajón, meter todos tus cambios sin acabar dentro, cerrar el cajón, y dejar tu escritorio limpio. Cuando vuelvas, abres el cajón y tienes todo exactamente donde lo dejaste.

```
Trabajo a medias  →  git stash  →  Working directory limpio
                                           ↓
                              (cambias de rama, arreglas el bug)
                                           ↓
                   Trabajo a medias  ←  git stash pop
```

---

## Los comandos de stash

| Comando | ¿Qué hace? |
|---------|------------|
| `git stash` | Guarda todos los cambios en el cajón |
| `git stash push -m "descripción"` | Guarda con un nombre descriptivo |
| `git stash list` | Ver todo lo que hay en el cajón |
| `git stash pop` | Saca el último stash y lo borra del cajón |
| `git stash apply` | Aplica el último stash pero **no** lo borra del cajón |
| `git stash apply stash@{2}` | Aplica un stash concreto por su índice |
| `git stash drop stash@{0}` | Borra un stash sin aplicarlo |
| `git stash clear` | Vacía todo el cajón (¡cuidado, sin vuelta atrás!) |

---

## 🎯 El Reto

### Simula la situación del bug urgente

**1. Empieza a trabajar en algo (sin acabar):**

```bash
git switch -c feature/nueva-funcionalidad
echo "funcionalidad a medias..." > nueva-funcionalidad.txt
echo "más cambios sin terminar" >> README.md
git status
# Verás dos archivos modificados
```

**2. Llega el bug urgente. Guarda tu trabajo en el stash:**

```bash
git stash push -m "WIP: nueva funcionalidad a medias"
git status
# El working directory está limpio ahora
```

**3. Cambia de rama para arreglar el bug:**

```bash
git switch main
echo "bug arreglado" > bugfix.txt
git add bugfix.txt
git commit -m "fix: arreglar bug urgente de producción"
```

**4. Vuelve a tu rama y recupera tu trabajo:**

```bash
git switch feature/nueva-funcionalidad
git stash list
# Verás tu stash guardado con el mensaje
git stash pop
git status
# Tus dos archivos modificados han vuelto
```

**5. Experimenta con varios stashes:**

```bash
# Crea un segundo stash
git stash push -m "experimento 2"
git stash list
# Verás stash@{0} y stash@{1}

# Aplica un stash concreto sin borrarlo
git stash apply stash@{1}

# Limpia la lista
git stash clear
git stash list
# Lista vacía
```

---

## 💡 Pro Tip: Stash incluye solo los tracked files por defecto

Por defecto, `git stash` guarda los archivos que Git ya conoce (tracked). Los archivos nuevos que nunca has añadido con `git add` **no se guardan**.

Para incluirlos también:

```bash
git stash push -u -m "incluye archivos nuevos también"
# La flag -u (--include-untracked) los incluye
```

Y si quieres guardar también los archivos en `.gitignore` (raro pero existe):

```bash
git stash push -a -m "todo incluido"
# La flag -a (--all) incluye absolutamente todo
```

---

## ✅ ¿Cómo sé que lo he hecho bien?

Al final, `git stash list` debe estar vacío y `git status` debe mostrar "nothing to commit, working tree clean". Habrás simulado el flujo completo: interrumpir trabajo, arreglar algo urgente y retomar donde lo dejaste.
