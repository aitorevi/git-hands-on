# 🍒 09 - Cherry-pick: Robar Commits Selectivamente

## Teoría ELI5: ¿Qué es cherry-pick?

Imagina que tu compañero/a tiene una rama con diez commits, pero tú solo necesitas uno de ellos: el que arregla un bug que también te afecta a ti. No quieres hacer merge de toda su rama (que tiene cosas a medias), solo quieres ese commit concreto.

**`git cherry-pick`** hace exactamente eso: copia un commit de cualquier rama y lo aplica en la tuya, como si lo hubieras hecho tú mismo. El nombre viene de "coger solo las cerezas que quieres" del árbol.

```
feature/x:   A ── B ── C ── D
                        ↑
                   "Solo quiero este"

main:        X ── Y ── Z ── C'  ← cherry-pick copia C aquí
```

> ⚠️ **Nota importante:** `cherry-pick` crea un commit nuevo con el mismo contenido pero un hash diferente. El commit original sigue donde estaba. Son dos commits distintos con los mismos cambios.

---

## ¿Cuándo usar cherry-pick?

| Situación | ¿Usar cherry-pick? |
|-----------|-------------------|
| Necesitas un bugfix de otra rama en la tuya | ✅ Sí |
| Quieres traer toda una funcionalidad de otra rama | ❌ Mejor merge o rebase |
| Un commit fue a la rama equivocada | ✅ Sí |
| Quieres sincronizar dos ramas de release | ✅ Sí |
| Lo usas todo el tiempo como flujo habitual | ⚠️ Señal de que el flujo de ramas no está bien diseñado |

---

## 🎯 El Reto

### Preparación: crea el escenario

**1. Crea una rama con varios commits, uno de los cuales nos interesa:**

```bash
git switch -c feature/varios-commits
echo "commit A: preparación" > archivo-a.txt && git add . && git commit -m "chore: commit A de preparación"
echo "fix: el bug que nos interesa" > bugfix-importante.txt && git add . && git commit -m "fix: corregir cálculo de fechas"
echo "commit C: experimento" > archivo-c.txt && git add . && git commit -m "feat: experimento sin acabar"
git log --oneline
```

Anota el hash del commit `fix: corregir cálculo de fechas` (el del medio).

**2. Vuelve a main y aplica solo ese commit:**

```bash
git switch main
git log --oneline
# Verás que el bugfix NO está aquí
```

**3. Aplica el cherry-pick con el hash que anotaste:**

```bash
git cherry-pick <hash-del-commit-fix>
git log --oneline
# Ahora el bugfix SÍ está en main, con un hash nuevo
```

**4. Comprueba que solo ese cambio llegó:**

```bash
ls
# Solo debe aparecer bugfix-importante.txt, no archivo-a.txt ni archivo-c.txt
```

---

### Bonus: cherry-pick de un rango de commits

```bash
# Aplica todos los commits desde hash1 hasta hash2 (sin incluir hash1)
git cherry-pick hash1..hash2

# Aplica varios commits sueltos a la vez
git cherry-pick hashA hashB hashC
```

---

## 💡 Pro Tip: Si el cherry-pick genera un conflicto

Cherry-pick puede producir conflictos exactamente igual que un merge. Si ocurre:

```bash
# Git se detiene y te muestra el conflicto
# Resuelve el conflicto en VS Code (igual que en el ejercicio 04)
git add archivo-conflictivo.txt

# Continúa el cherry-pick
git cherry-pick --continue

# O si prefieres abortar y dejarlo todo como estaba
git cherry-pick --abort
```

---

## ✅ ¿Cómo sé que lo he hecho bien?

En `main`, `git log --oneline` debe mostrar el commit del bugfix con un hash diferente al que tiene en `feature/varios-commits`. El archivo `bugfix-importante.txt` debe existir en `main`, pero `archivo-a.txt` y `archivo-c.txt` no.
