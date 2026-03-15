🇬🇧 [Read in English](README.md)
# 🔍 Web Data Exposure Scanner (WDES)

<p align="center">
  <img src="https://img.shields.io/badge/version-1.1.0-blue.svg" alt="Version">
  <img src="https://img.shields.io/badge/python-3.8+-green.svg" alt="Python">
  <img src="https://img.shields.io/badge/license-MIT-orange.svg" alt="License">
  <img src="https://img.shields.io/badge/platform-linux%20%7C%20windows%20%7C%20macos-lightgrey.svg" alt="Platform">
  <img src="https://img.shields.io/badge/tor-compatible-purple.svg" alt="Tor Compatible">
</p>

**WDES** es una herramienta OSINT (Open Source Intelligence) diseñada para detectar datos sensibles expuestos en sitios web. Permite identificar emails, documentos de identidad, números de teléfono y archivos potencialmente sensibles que puedan estar accesibles públicamente.

## ⚠️ Aviso legal y uso ético

Esta herramienta está diseñada **ÚNICAMENTE** para:

- ✅ Auditorías de seguridad autorizadas
- ✅ Evaluación de tu propia infraestructura
- ✅ Investigación con permiso explícito del propietario del sitio
- ✅ Programas de Bug Bounty autorizados
- ✅ Fines educativos en entornos controlados

**El uso no autorizado puede violar leyes locales e internacionales.** El autor no se responsabiliza por el mal uso de esta herramienta.

## 🚀 Características

- 📧 **Detección de emails** expuestos en páginas web
- 🪪 **Detección de documentos de identidad** con patrones para múltiples países:
  - 🇺🇾 Uruguay (Cédula de Identidad)
  - 🇦🇷 Argentina (DNI)
  - 🇧🇷 Brasil (CPF)
  - 🇨🇱 Chile (RUT)
  - 🇲🇽 México (CURP)
  - 🇨🇴 Colombia (Cédula de Ciudadanía)
  - 🇵🇪 Perú (DNI)
  - 🇪🇸 España (DNI/NIE)
  - 🌐 Patrón genérico personalizable
- 📞 **Detección de números de teléfono**
- 📁 **Identificación de archivos sensibles** (.pdf, .doc, .xls, .sql, .bak, etc.)
- 🧅 **Soporte para red Tor** (conexión anónima opcional)
- 🔄 **Crawling recursivo** con control de profundidad
- 🚀 **Multi-threading** para escaneos rápidos
- 📊 **Reportes en JSON y TXT**
- 🎨 **Interfaz colorida** y amigable
- 💻 **Modo interactivo y CLI**

## 📋 Requisitos

- Python 3.8 o superior
- Dependencias:
  ```
  requests
  beautifulsoup4
  colorama
  tqdm
  PySocks (opcional, solo para Tor)
  ```
- **Para usar Tor:** Servicio Tor corriendo en el puerto 9050

## 🔧 Instalación

### Opción 1: Instalación rápida

```bash
# Clonar el repositorio
git clone https://github.com/eduardoit/web-data-exposure-scanner.git
cd web-data-exposure-scanner

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar
python scanner.py
```

### Opción 2: Instalación manual

```bash
# Instalar dependencias manualmente
pip install requests beautifulsoup4 colorama tqdm

# Opcional: soporte para Tor
pip install PySocks

# Descargar el script
wget https://raw.githubusercontent.com/eduardoit/web-data-exposure-scanner/main/scanner.py

# Dar permisos de ejecución (Linux/Mac)
chmod +x scanner.py

# Ejecutar
python scanner.py
```

### Configurar Tor (Opcional)

```bash
# Ubuntu/Debian
sudo apt install tor
sudo systemctl start tor

# macOS (con Homebrew)
brew install tor
brew services start tor

# Verificar que Tor está corriendo
curl --socks5-hostname localhost:9050 https://check.torproject.org/api/ip
```

## 📖 Uso

### Modo Interactivo (Recomendado para principiantes)

```bash
python scanner.py
```

El modo interactivo te guiará paso a paso:

1. **Selecciona el modo de conexión** (Directa o Tor)
2. Ingresa la URL del sitio objetivo
3. Selecciona los patrones de documentos a buscar
4. Configura opciones avanzadas (opcional)
5. El escaneo iniciará automáticamente

### Modo Línea de Comandos

```bash
# Escaneo básico
python scanner.py -u ejemplo.com

# Escaneo anónimo a través de Tor
python scanner.py -u ejemplo.com --tor

# Con múltiples patrones de documentos
python scanner.py -u ejemplo.com -p uruguay,argentina,brasil

# Configuración personalizada
python scanner.py -u ejemplo.com -d 5 -m 200 -t 10

# Guardar reporte
python scanner.py -u ejemplo.com -o reporte.json

# Ver todos los patrones disponibles
python scanner.py --list-patterns
```

### Opciones de Línea de Comandos

| Opción | Descripción | Default |
|--------|-------------|---------|
| `-u, --url` | URL del sitio objetivo | - |
| `-p, --patterns` | Patrones de documentos (separados por coma) | uruguay |
| `-d, --depth` | Profundidad máxima de crawling | 3 |
| `-m, --pages` | Máximo de páginas a escanear | 100 |
| `-t, --threads` | Threads concurrentes | 5 |
| `-o, --output` | Archivo de salida (JSON) | - |
| `--tor` | Usar red Tor para anonimato | False |
| `--no-ssl` | Deshabilitar verificación SSL | False |
| `-q, --quiet` | Modo silencioso | False |
| `--list-patterns` | Listar patrones disponibles | - |

## 🧅 Uso con Tor

### Ventajas
- Tu IP real no queda en los logs del sitio objetivo
- Útil para OSINT donde no querés dejar rastro
- Rotación automática de IP
- Evita rate-limiting basado en IP

### Consideraciones
- **Velocidad reducida**: El escaneo será más lento
- **Bloqueos**: Algunos sitios bloquean tráfico de Tor
- **CAPTCHAs**: Cloudflare y otros pueden mostrar CAPTCHAs
- **Threads reducidos**: Automáticamente se limitan a 3 para no sobrecargar la red Tor

### Ejemplo de flujo con Tor

```
┌─ Modo de Conexión ────────────────────────────────────────────────────┐
│                                                                       │
│  1. 🌐 Conexión directa (más rápido)                                 │
│  2. 🧅 Conexión a través de Tor (anónimo)                            │
│                                                                       │
│ Seleccione modo de conexión [1/2]: 2                                 │
│                                                                       │
│  ⏳ Verificando conexión a Tor...                                    │
│  ✓ Conectado a Tor. IP de salida: 185.220.101.xxx                    │
│                                                                       │
│  ⚠️  Nota: El escaneo será más lento a través de Tor                 │
│  ⚠️  Algunos sitios pueden bloquear tráfico de Tor                   │
└───────────────────────────────────────────────────────────────────────┘
```

## 📊 Ejemplo de Salida

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                    RESUMEN DEL ESCANEO                                       ║
╚══════════════════════════════════════════════════════════════════════════════╝

  Objetivo: https://ejemplo.com
  Fecha: 2024-01-15T10:30:00
  URLs escaneadas: 87

  HALLAZGOS:
  ────────────────────────────────────────
  📧 Emails únicos encontrados: 23
      • contacto@ejemplo.com
      • admin@ejemplo.com
      • ...
  
  🪪 Cédula de Identidad (Uruguay): 5
      • 1.234.567-8
      • 2.345.678-9
      • ...
  
  📁 Archivos interesantes: 12
      • https://ejemplo.com/docs/reporte.pdf
      • https://ejemplo.com/data/usuarios.xlsx
      • ...
```

## 📁 Estructura del Reporte JSON

```json
{
  "target": "https://ejemplo.com",
  "scan_date": "2024-01-15T10:30:00",
  "summary": {
    "total_urls_scanned": 87,
    "unique_emails_found": 23,
    "unique_documents_found": 5,
    "unique_phones_found": 8,
    "interesting_files_found": 12
  },
  "findings": {
    "emails": ["contacto@ejemplo.com", "..."],
    "documents": {
      "Cédula de Identidad (Uruguay)": ["1.234.567-8", "..."]
    },
    "phones": ["+598 99 123 456", "..."],
    "interesting_files": ["https://ejemplo.com/doc.pdf", "..."]
  }
}
```

## 🔐 Casos de Uso Legítimos

### 1. Auditoría de tu propia organización
```bash
python scanner.py -u miempresa.com -p uruguay -d 5 -m 500 -o auditoria_miempresa.json
```

### 2. Verificación pre-lanzamiento
```bash
python scanner.py -u staging.miapp.com -p generic -o pre_launch_check.json
```

### 3. Monitoreo periódico de exposición
```bash
# Agregar a cron para escaneos semanales
0 0 * * 0 python /path/to/scanner.py -u miempresa.com -o /logs/scan_$(date +\%Y\%m\%d).json -q
```

## 🛠️ Personalización

### Agregar nuevos patrones de documentos

Edita la variable `ID_PATTERNS` en el script:

```python
ID_PATTERNS = {
    "mi_pais": {
        "name": "Documento de Mi País",
        "pattern": r'\b\d{8}-[A-Z]\b',  # Tu regex aquí
        "example": "12345678-A",
        "description": "8 dígitos + guión + letra"
    },
    # ... otros patrones
}
```

### Personalizar extensiones de archivos

Edita la variable `INTERESTING_EXTENSIONS`:

```python
INTERESTING_EXTENSIONS = [
    '.pdf', '.doc', '.docx', '.xls', '.xlsx', 
    '.csv', '.txt', '.json', '.xml', '.sql', 
    '.bak', '.log', '.conf', '.env',
    # Agrega más extensiones según necesites
]
```

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Por favor:

1. Fork el repositorio
2. Crea una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -am 'Agrega nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

### Ideas para contribuir

- [ ] Agregar más patrones de documentos de otros países
- [ ] Implementar detección de números de tarjetas de crédito (con precaución)
- [ ] Añadir exportación a CSV/Excel
- [ ] Crear interfaz web
- [ ] Agregar integración con APIs de breach databases
- [ ] Implementar rate limiting inteligente
- [ ] Soporte para autenticación (cookies, tokens)

## 📜 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

## 🙏 Agradecimientos

- A la comunidad de seguridad por compartir conocimiento
- A todos los que contribuyen a hacer internet más seguro

## 📧 Contacto

Si encuentras bugs o tienes sugerencias, por favor abre un [Issue](https://github.com/eduardoit/web-data-exposure-scanner/issues).

---

<p align="center">
  <strong>Hecho por <a href="https://github.com/eduardoit">eduardoit</strong>
</p>

<p align="center">
  <em>Recuerda: Con grandes poderes vienen grandes responsabilidades. Usa esta herramienta de forma ética.</em>
</p>
