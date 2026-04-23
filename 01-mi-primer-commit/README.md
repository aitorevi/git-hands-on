# 📸 01 - Mi Primer Commit

## Teoría ELI5: ¿Qué es el Staging Area?

Imagina que estás preparando una mudanza. Tienes tres habitaciones llenas de cosas (tus archivos modificados), pero no puedes meter todo en una caja a la vez. Primero seleccionas qué va en **esta** caja (eso es el `git add`), le pones la etiqueta de qué hay dentro (eso es el mensaje del `git commit`), y la sellas (eso es el commit en sí).

El **Staging Area** (o "índice") es esa zona intermedia donde preparas exactamente qué cambios van a ir en el próximo commit. Esto es importante porque te permite hacer commits precisos y con sentido, en lugar de guardar todo de golpe sin pensar.

```
[Tus archivos]  →  git add  →  [Staging Area]  →  git commit  →  [Historial]
  (Working           (Preparas         (La caja          (La caja
   Directory)         la caja)          sellada)          archivada)
```

### ¿Por qué importa el mensaje del commit?

Un commit es inútil si no sabes qué hay dentro. Compara estos dos mensajes:

- ❌ `git commit -m "cambios"`
- ✅ `git commit -m "feat: añadir validación de email en formulario de registro"`

El segundo mensaje, dentro de seis meses, te dice exactamente qué pasó y por qué. El primero no te dice nada. **Un buen mensaje de commit es una carta para tu yo del futuro** (y para tus compañeros/as).

---

## 🎯 El Reto

Sigue estos pasos uno a uno. Después de cada comando, ejecuta `git status` para ver cómo cambia el estado.

**1. Crea un archivo nuevo en esta carpeta:**
```bash
echo "Hola, soy [tu nombre]" > presentacion.txt
```

**2. Comprueba que Git lo ha detectado (aparecerá como "untracked"):**
```bash
git status
```

**3. Añade el archivo al Staging Area:**
```bash
git add presentacion.txt
```

**4. Vuelve a comprobar el estado. ¿Ves la diferencia?**
```bash
git status
```

**5. Haz el commit con un mensaje descriptivo:**
```bash
git commit -m "docs: añadir presentación de [tu nombre]"
```

**6. Verifica que el commit se ha guardado en el historial:**
```bash
git log --oneline
```

---

## 💡 Pro Tip: La convención de mensajes

En equipos profesionales se suele usar una convención llamada **Conventional Commits**. Los prefijos más comunes son:

| Prefijo | Cuándo usarlo |
|---------|--------------|
| `feat:` | Añades una funcionalidad nueva |
| `fix:` | Corriges un bug |
| `docs:` | Cambios solo en documentación |
| `refactor:` | Reorganizas código sin cambiar lo que hace |
| `chore:` | Tareas de mantenimiento (dependencias, config) |

No es obligatorio, pero cuando lo ves en un proyecto, sabes al instante de qué va cada cambio sin abrir el código.

---

## ✅ ¿Cómo sé que lo he hecho bien?

Ejecuta `git log --oneline` y deberías ver tu commit en la primera línea del historial. ¡Enhorabuena, acabas de dominar el ciclo básico de Git!
