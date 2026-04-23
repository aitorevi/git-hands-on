# 🌌 02 - Ramas y Universos Paralelos

## Teoría ELI5: ¿Por qué existen las ramas?

Piensa en las ramas como **universos paralelos** de tu proyecto. La rama principal (`main`) es el universo "oficial" y estable. Cuando quieres añadir una nueva funcionalidad o arreglar un bug, creas un universo paralelo (una rama nueva) donde experimentas con total libertad.

Si el experimento sale bien, **fusionas** ese universo con el principal (merge/pull request). Si sale mal, simplemente borras esa rama y el universo principal nunca se ve afectado.

```
main:         A ── B ── C ──────────────── F  (merge)
                         \                /
mi-feature:               D ── E ─────────
```

Esto tiene dos ventajas enormes:
1. **El código que funciona en `main` nunca se rompe** mientras trabajas en algo nuevo.
2. **Varias personas pueden trabajar en paralelo** sin pisarse.

### La rama `main` es sagrada

En un equipo real, nadie empuja código directamente a `main`. Todo el mundo trabaja en sus propias ramas y el código entra a `main` solo a través de revisiones de código (Pull Requests). Respeta siempre esta norma.

---

## 🎯 El Reto

**1. Primero, comprueba en qué rama estás ahora mismo:**
```bash
git branch
```
Verás un asterisco `*` junto a la rama activa.

**2. Crea una rama nueva con tu nombre y salta a ella en un solo comando:**
```bash
git switch -c feature/[tu-nombre]-saludo
```

**3. Confirma que el cambio ha funcionado:**
```bash
git branch
```

**4. Crea un archivo en esta nueva rama:**
```bash
echo "Hola desde la rama de [tu nombre]!" > saludo.txt
```

**5. Haz un commit de ese cambio:**
```bash
git add saludo.txt
git commit -m "feat: añadir saludo de [tu nombre]"
```

**6. Vuelve a la rama principal y comprueba que el archivo NO está ahí:**
```bash
git switch main
ls
```

¿Ves? El archivo `saludo.txt` ha "desaparecido". No se ha borrado, simplemente está en el otro universo. Vuelve a tu rama y reaparecerá:

```bash
git switch feature/[tu-nombre]-saludo
ls
```

---

## 💡 Pro Tip: Nombra tus ramas con sentido

Al igual que los commits, el nombre de una rama debe decir qué hay dentro. Estos son los patrones más comunes en equipos:

| Patrón | Ejemplo |
|--------|---------|
| `feature/descripcion` | `feature/login-con-google` |
| `fix/descripcion` | `fix/error-404-en-perfil` |
| `hotfix/descripcion` | `hotfix/fallo-critico-pago` |
| `chore/descripcion` | `chore/actualizar-dependencias` |

Evita nombres como `mi-rama`, `prueba`, `arreglado` o `v2`. Son inútiles en un historial con 50 ramas.

---

## ✅ ¿Cómo sé que lo he hecho bien?

Ejecuta `git log --oneline` estando en tu rama de feature. Deberías ver el commit que creaste. Luego haz `git switch main` y ejecuta `git log --oneline` de nuevo. Ese commit no debería aparecer en `main`. ¡Perfecto, ya entiendes el aislamiento de ramas!
