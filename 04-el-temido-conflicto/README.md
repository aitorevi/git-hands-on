# ⚔️ 04 - El Temido Conflicto (y Cómo Sobrevivir a Él)

## Teoría ELI5: ¿Por qué ocurren los conflictos?

Un conflicto ocurre cuando **dos personas modifican la misma línea del mismo archivo** y Git no sabe cuál de las dos versiones debe quedarse. Git es muy listo, pero no puede leer la mente: cuando llega a esa situación, se detiene y te pide que decidas tú.

No hay nada que temer. Un conflicto es simplemente Git diciéndote: **"Oye, aquí hay una decisión que debo delegarte a ti"**.

Cuando ocurre un conflicto, Git añade marcadores en el archivo:

```
<<<<<<< HEAD
Esta es tu versión del código (la que tienes tú)
=======
Esta es la versión que viene de la otra rama
>>>>>>> feature/otra-rama
```

Tu trabajo es **eliminar los marcadores** y dejar solo el código que debe quedar (puede ser uno, el otro, o una combinación de ambos).

---

## 🎯 El Reto: Provocar un conflicto a propósito

Vamos a recrear la situación más común en un equipo: dos personas editan el mismo archivo.

### Paso 1: Preparar el escenario

**Desde `main`, crea un archivo compartido:**
```bash
git switch main
echo "El color favorito del equipo es: azul" > color-favorito.txt
git add color-favorito.txt
git commit -m "docs: añadir archivo de color favorito del equipo"
```

### Paso 2: Crear la primera rama y modificar el archivo

```bash
git switch -c feature/color-rojo
```

Abre `color-favorito.txt` y cambia la línea para que diga `rojo`. Luego:

```bash
git add color-favorito.txt
git commit -m "feat: cambiar color favorito a rojo"
```

### Paso 3: Volver a `main` y crear otra versión diferente

```bash
git switch main
```

Abre `color-favorito.txt` y cambia la línea para que diga `verde`. Luego:

```bash
git add color-favorito.txt
git commit -m "feat: cambiar color favorito a verde"
```

### Paso 4: Intentar fusionar. Aquí viene el conflicto

```bash
git merge feature/color-rojo
```

Verás un mensaje parecido a este:
```
Auto-merging color-favorito.txt
CONFLICT (content): Merge conflict in color-favorito.txt
Automatic merge failed; fix conflicts and then commit the result.
```

¡Enhorabuena! Has provocado un conflicto. Ahora `git status` te mostrará el archivo como "both modified".

### Paso 5: Resolver el conflicto en VS Code

**Abre el archivo** `color-favorito.txt` en VS Code. Verás los marcadores de conflicto y, encima de ellos, VS Code te ofrecerá botones de acción:

- **"Accept Current Change"** → Se queda con tu versión (verde)
- **"Accept Incoming Change"** → Se queda con la versión que viene (rojo)
- **"Accept Both Changes"** → Añade las dos líneas
- **"Compare Changes"** → Te muestra un diff visual

Para este ejercicio, haz clic en **"Accept Current Change"** para quedarte con `verde`.

### Paso 6: Marcar el conflicto como resuelto y hacer commit

Una vez editado el archivo (ya sin marcadores `<<<<<<<`), debes decirle a Git que has terminado:

```bash
git add color-favorito.txt
git commit -m "fix: resolver conflicto de color favorito, se mantiene verde"
```

### Paso 7: Comprobar el resultado

```bash
git log --oneline
```

Verás un "merge commit" en el historial. ¡El conflicto ha sido resuelto!

---

## 💡 Pro Tip: La mejor estrategia ante un conflicto

Cuando te encuentres un conflicto en un proyecto real, sigue este protocolo:

1. **No entres en pánico.** Es normal y tiene solución siempre.
2. **Habla con la persona** que hizo el otro cambio. El conflicto es técnico, pero la decisión puede ser de negocio. ¿Qué versión tiene más sentido? No lo decidas solo/a.
3. **Nunca borres código de otro sin entenderlo.** A veces la solución es mezclar ambas versiones.
4. **Después de resolver, ejecuta los tests.** Asegúrate de que el código que queda sigue funcionando.

Los conflictos frecuentes son una señal de que el equipo necesita mejor comunicación, no de que alguien haya hecho algo mal.

---

## ✅ ¿Cómo sé que lo he hecho bien?

Ejecuta `git log --oneline`. Deberías ver el "merge commit" como el más reciente. El archivo `color-favorito.txt` debe contener solo la versión final elegida, sin ningún marcador `<<<<<<<`, `=======`, ni `>>>>>>>`. Si ves alguno de esos símbolos, el conflicto no está resuelto todavía.

---

## 🏁 ¡Has completado el taller!

Si has llegado hasta aquí, ya sabes:
- ✅ Qué es el ciclo add → commit y por qué importa
- ✅ Trabajar con ramas para aislar tu trabajo
- ✅ Sincronizar con el remoto y crear Pull Requests
- ✅ Resolver conflictos sin perder la calma

El siguiente paso es practicar en proyectos reales. La única forma de interiorizar Git es usarlo todos los días.
