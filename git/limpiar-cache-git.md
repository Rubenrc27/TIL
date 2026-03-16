# Limpiar la caché de Git para archivos en .gitignore

**Fecha:** 13 de marzo de 2026

A veces, después de añadir un archivo al `.gitignore`, Git lo sigue rastreando porque ya estaba en el índice. Para solucionar esto, es necesario limpiar la caché del repositorio.

## Pasos para limpiar la caché

**1. Eliminar archivos del índice (sin borrar los archivos físicos):**
```bash
git rm -r --cached .
```

**2. Volver a añadir todos los archivos:**
Este paso añadirá de nuevo todos los archivos, pero respetando las reglas actualizadas de `.gitignore`.
```bash
git add .
```

**3. Realizar un commit con el cambio:**
```bash
git commit -m "Limpiar caché de archivos ignorados"
```

Esto forzará a Git a ignorar los archivos definidos en el `.gitignore`.
