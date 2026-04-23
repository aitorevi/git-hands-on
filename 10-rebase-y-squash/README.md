# 🔄 10 - Rebase y Squash: Reescribir la Historia

## Teoría ELI5: Merge vs Rebase

Cuando quieres integrar los cambios de una rama en otra tienes dos estrategias:

**Merge** une dos ramas creando un commit especial de fusión. El historial queda con su forma de árbol, con bifurcaciones y merges visibles. Es honesto: muestra exactamente lo que pasó.

**Rebase** "replanta" tus commits sobre la punta de otra rama. El resultado parece que nunca hubo una bifurcación: es un historial completamente lineal. Es más limpio, pero reescribe la historia.

```
ANTES del rebase:
main:      A ── B ── C
                \
feature:         D ── E

DESPUÉS de git rebase main (desde feature):
main:      A ── B ── C
                          \
feature:                   D' ── E'   ← commits nuevos, mismos cambios
```

> Los commits D y E se han "recreado" como D' y E' con nuevos hashes. El contenido es idéntico, pero la historia ha cambiado.

---

## ⚠️ La regla de oro del rebase

> **Nunca hagas rebase de commits que ya están en el repositorio remoto y que otros compañeros/as pueden haber descargado.**

Rebase reescribe hashes. Si otra persona tiene los hashes viejos y tú los cambias con un rebase, sus ramas quedan huérfanas y tendrán problemas al sincronizar. Rebase es seguro **solo en tu rama local antes de hacer push**.

---

## Squash: Limpiar el historial antes de mergear

Durante el desarrollo es normal hacer commits de trabajo en progreso:

```
abc1234 feat: inicio del formulario
def5678 fix: olvidé añadir validación
ghi9012 chore: quitar console.log
jkl3456 fix: ahora sí funciona
```

Ese historial es ruidoso. Con **squash** aplastas todos esos commits en uno solo y limpias el mensaje antes de que llegue a `main`:

```
xyz9999 feat: añadir formulario de contacto con validación
```

---

## 🎯 El Reto

### Parte 1: Rebase para mantener el historial lineal

**1. Crea una situación donde main ha avanzado mientras trabajabas:**

```bash
# Crea tu rama de feature
git switch -c feature/rebase-prueba
echo "mi trabajo" > mi-trabajo.txt
git add . && git commit -m "feat: mi trabajo inicial"

# Simula que main ha avanzado (otro compañero ha mergeado algo)
git switch main
echo "avance en main" > avance-main.txt
git add . && git commit -m "feat: avance del equipo en main"

# Vuelve a tu rama
git switch feature/rebase-prueba
git log --oneline --graph --all
# Verás que la historia se ha bifurcado
```

**2. Aplica el rebase para poner tu trabajo encima de main:**

```bash
git rebase main
git log --oneline --graph --all
# Ahora el historial es lineal: A─B─C─D, sin bifurcaciones
```

---

### Parte 2: Squash con rebase interactivo

**1. Crea varios commits "sucios" de trabajo en progreso:**

```bash
git switch -c feature/squash-prueba
echo "v1" > feature.txt && git add . && git commit -m "WIP: empezando"
echo "v2" >> feature.txt && git add . && git commit -m "fix: faltaba algo"
echo "v3" >> feature.txt && git add . && git commit -m "chore: quitar debug"
echo "v4" >> feature.txt && git add . && git commit -m "fix: ahora sí"
git log --oneline
# Verás los 4 commits
```

**2. Aplasta los 4 commits en 1 con rebase interactivo:**

```bash
git rebase -i HEAD~4
```

Se abrirá tu editor de texto con algo así:

```
pick abc1111 WIP: empezando
pick def2222 fix: faltaba algo
pick ghi3333 chore: quitar debug
pick jkl4444 fix: ahora sí
```

**3. Cambia `pick` por `squash` (o `s`) en todos menos el primero:**

```
pick abc1111 WIP: empezando
squash def2222 fix: faltaba algo
squash ghi3333 chore: quitar debug
squash jkl4444 fix: ahora sí
```

**4. Guarda y cierra el editor.** Git abrirá otro editor para que escribas el mensaje del commit final. Borra todo y escribe uno limpio:

```
feat: añadir feature con validación completa
```

**5. Guarda y verifica el resultado:**

```bash
git log --oneline
# Verás un solo commit limpio en lugar de los cuatro originales
```

---

## 💡 Pro Tip: `fixup` en lugar de `squash`

En el rebase interactivo también puedes usar `fixup` (o `f`). La diferencia:

- `squash` → te pregunta qué mensaje quieres para el commit resultante
- `fixup` → descarta el mensaje del commit y usa el del primero sin preguntar

Si tienes varios commits de "fix: typo" que claramente deben aplastarse sin más, `fixup` es más rápido.

```
pick abc1111 feat: nueva pantalla de login
fixup def2222 fix: typo en el botón
fixup ghi3333 fix: margen mal puesto
```

---

## ✅ ¿Cómo sé que lo he hecho bien?

Tras el squash, `git log --oneline` debe mostrar un solo commit con el mensaje limpio que escribiste. El contenido del archivo `feature.txt` debe ser el de la versión final (v4). Los cuatro commits originales han desaparecido del historial.
