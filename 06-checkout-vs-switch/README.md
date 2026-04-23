# 🔀 06 - Checkout vs Switch: La Vieja Guardia y el Nuevo Orden

## Teoría ELI5: ¿Por qué existen los dos?

`git checkout` es uno de los comandos más antiguos de Git y hacía **demasiadas cosas a la vez**: cambiar de rama, restaurar archivos, crear ramas nuevas... Era confuso porque dependiendo de los argumentos hacía cosas completamente distintas.

En Git 2.23 (2019) se introdujeron dos comandos nuevos para dividir esa responsabilidad:

- **`git switch`** → solo para moverse entre ramas
- **`git restore`** → solo para restaurar archivos

`git checkout` sigue existiendo y seguirás viéndolo en proyectos, tutoriales y Stack Overflow. Por eso necesitas entender los dos.

---

## Tabla de equivalencias

| Lo que quieres hacer | Forma antigua (`checkout`) | Forma moderna (`switch` / `restore`) |
|----------------------|---------------------------|--------------------------------------|
| Cambiar a una rama existente | `git checkout main` | `git switch main` |
| Crear una rama y saltar | `git checkout -b feature/x` | `git switch -c feature/x` |
| Descartar cambios en un archivo | `git checkout -- archivo.txt` | `git restore archivo.txt` |
| Descartar **todos** los cambios | `git checkout -- .` | `git restore .` |
| Ver un commit antiguo (modo lectura) | `git checkout abc1234` | `git checkout abc1234` *(aquí sigue igual)* |

> 💡 **Recomendación:** usa `switch` y `restore` en tu trabajo diario. Son más claros y explícitos. Usa `checkout` solo cuando leas documentación antigua o cuando necesites inspeccionar un commit concreto.

---

## El "Detached HEAD": el caso especial de `checkout`

Cuando haces `git checkout <hash-de-commit>` (no una rama, sino un commit concreto), Git te pone en modo **"Detached HEAD"**. Esto significa que estás viendo el proyecto tal como estaba en ese momento exacto, pero no estás en ninguna rama.

```
main:   A ── B ── C ── D  ← main
                  ↑
             HEAD (detached)
             "Estás aquí, flotando"
```

Es como leer una foto del pasado. Si haces commits en este estado, esos commits se pierden en cuanto cambias de rama (a no ser que crees una rama nueva desde ahí).

```bash
# Ver cómo estaba el proyecto hace 3 commits (solo lectura)
git checkout HEAD~3

# Volver a la rama normal
git switch main
```

---

## 🎯 El Reto

**1. Practica las equivalencias básicas:**

```bash
# Crea una rama de prueba con la forma moderna
git switch -c prueba/checkout-vs-switch

# Vuelve a main con la forma antigua
git checkout main

# Vuelve a la rama con la forma antigua
git checkout prueba/checkout-vs-switch

# Vuelve a main con la forma moderna
git switch main
```

**2. Practica `restore` para descartar cambios:**

```bash
# Modifica cualquier archivo sin hacer commit
echo "cambio temporal" >> README.md
git status
# Verás el archivo modificado

# Descarta el cambio con restore
git restore README.md
git status
# El archivo ha vuelto a su estado original
```

**3. Inspecciona un commit antiguo con Detached HEAD:**

```bash
# Mira el hash de un commit antiguo
git log --oneline

# Salta a ese commit (sustituye el hash por uno real tuyo)
git checkout <hash>

# Observa el mensaje de aviso de "detached HEAD"
# Explora los archivos del proyecto en ese momento
ls

# Vuelve a main
git switch main
```

---

## 💡 Pro Tip: `git switch -` vuelve a la rama anterior

Igual que `cd -` en el terminal vuelve al directorio anterior, en Git tienes:

```bash
git switch -c feature/nueva-cosa
# ... trabajas un rato ...
git switch -   # Vuelve a la rama en la que estabas antes
```

Es un atajo muy útil cuando saltas entre dos ramas frecuentemente.

---

## ✅ ¿Cómo sé que lo he hecho bien?

Ejecuta `git log --oneline` y `git status` al final. Deberías estar en `main`, sin cambios pendientes y sin estar en modo "detached HEAD" (el prompt de Git o `git status` te avisará si lo estuvieras).
