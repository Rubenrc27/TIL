# Instalar DBeaver en Ubuntu usando el repositorio APT

**Fecha:** 2 de marzo de 2026

Hoy aprendí cómo instalar DBeaver Community Edition en Ubuntu utilizando su repositorio oficial APT.

## Pasos de instalación

**1. Descargar y guardar la llave de seguridad:**
```bash
curl -fsSL [https://dbeaver.io/debs/dbeaver.gpg.key](https://dbeaver.io/debs/dbeaver.gpg.key) | sudo gpg --dearmor -o /etc/apt/keyrings/dbeaver.gpg
```

**2. Añadir el repositorio oficial:**
```bash
echo "deb [signed-by=/etc/apt/keyrings/dbeaver.gpg] [https://dbeaver.io/debs/dbeaver-ce](https://dbeaver.io/debs/dbeaver-ce) /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
```

**3. Actualizar e instalar:**
```bash
sudo apt update && sudo apt install dbeaver-ce
```
