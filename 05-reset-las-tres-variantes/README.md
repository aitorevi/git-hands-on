# ⏪ 05 - Reset: Las Tres Variantes

## Teoría ELI5: ¿Qué es `git reset`?

`git reset` mueve el puntero de tu rama hacia atrás en el historial, como si "deshicieras" commits. El truco está en que tienes **tres modos** que controlan qué pasa con los cambios de esos commits deshhechos: ¿los conservas?, ¿los preparas para un nuevo commit?, ¿los borras para siempre?

```
HEAD~2        HEAD~1        HEAD (actual)
   A ────────── B ────────── C
                             ↑
                      git reset vuelve aquí
```

### Las tres variantes explicadas

| Modo | Comando | ¿Qué pasa con los cambios del commit deshecho? |
|------|---------|------------------------------------------------|
| 🟢 **Soft** | `git reset --soft HEAD~1` | Se quedan en el **Staging Area** (listos para hacer commit) |
| 🟡 **Mixed** | `git reset --mixed HEAD~1` | Se quedan en tu **Working Directory** (modificados pero sin preparar) |
| 🔴 **Hard** | `git reset --hard HEAD~1` | **Desaparecen para siempre**. El directorio queda exactamente como en ese commit |

> ⚠️ **`--hard` es el único peligroso.** Los otros dos son seguros porque tus cambios siguen existiendo. Con `--hard`, si no tienes backup, ese código se va para siempre.

---

## 🎯 El Reto

### Preparación: crea tres commits de prueba

```bash
echo "línea 1" > prueba.txt && git add . && git commit -m "feat: línea 1"
echo "línea 2" >> prueba.txt && git add . && git commit -m "feat: línea 2"
echo "línea 3" >> prueba.txt && git add . && git commit -m "feat: línea 3"
git log --oneline
```

Deberías ver los tres commits. Ahora experimenta con cada modo:

---

### Experimento 1: `--soft`

```bash
git reset --soft HEAD~1
git status
```

Observa: el commit de "línea 3" ha desaparecido del log, pero `prueba.txt` con la línea 3 sigue en el **Staging Area** (en verde en `git status`). Puedes hacer commit de nuevo inmediatamente.

```bash
git log --oneline
```

Rehaz el commit para continuar con los siguientes experimentos:
```bash
git commit -m "feat: línea 3 (rehecha)"
```

---

### Experimento 2: `--mixed` (el comportamiento por defecto)

```bash
git reset HEAD~1
git status
```

El commit desaparece y los cambios vuelven al **Working Directory** (en rojo en `git status`). Necesitas hacer `git add` antes de poder hacer commit.

Rehaz el commit para continuar:
```bash
git add prueba.txt
git commit -m "feat: línea 3 (rehecha de nuevo)"
```

---

### Experimento 3: `--hard`

```bash
git reset --hard HEAD~1
git status
cat prueba.txt
```

El commit ha desaparecido y el archivo ha vuelto a su estado anterior. La "línea 3" ya no existe en ningún sitio.

---

## 💡 Pro Tip: `--hard` tiene red de seguridad (por poco tiempo)

Si hiciste un `git reset --hard` por error, Git guarda un registro temporal llamado `reflog`. Durante unos días puedes recuperar el commit perdido:

```bash
git reflog
# Localiza el hash del commit perdido, por ejemplo: abc1234
git reset --hard abc1234
```

Pasado un tiempo (por defecto 90 días), Git limpia el reflog y el commit se pierde definitivamente. No cuentes con esto como plan habitual.

---

## ✅ ¿Cómo sé que lo he hecho bien?

Tras el experimento 3, ejecuta `git log --oneline`. Deberías ver solo los dos primeros commits. `cat prueba.txt` debe mostrar solo "línea 1" y "línea 2".
