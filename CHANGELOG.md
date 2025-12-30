# Changelog

Todos los cambios notables de este proyecto serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2024-12-29

### Agregado
- 🧅 **Soporte para red Tor** - Conexión anónima opcional con verificación automática
- 📊 **Reportes detallados múltiples** - Genera 4 tipos de archivos:
  - JSON completo
  - TXT general
  - Detalle de documentos con URLs de origen
  - Documentos únicos con conteo de ocurrencias
- 🔗 **Relación documento ↔ URL** - Cada CI encontrada se vincula con su URL de origen
- ⚠️ **Advertencias para patrones genéricos** - Alerta sobre posibles falsos positivos
- 🖼️ **Búsqueda en URLs de recursos** - Detecta documentos en nombres de archivos de imágenes, videos, etc.

### Mejorado
- Patrón de Uruguay más flexible (soporta múltiples formatos)
- Patrón genérico optimizado para reducir falsos positivos (excluye IDs con ceros iniciales)
- Menú interactivo más intuitivo con selección de modo de conexión al inicio
- Ajuste automático de threads y timeout cuando se usa Tor

## [1.0.0] - 2024-12-29

### Agregado
- 🔍 Scanner OSINT para detectar datos sensibles expuestos
- 📧 Detección de emails en páginas web
- 🪪 Detección de documentos de identidad con patrones para 8 países:
  - Uruguay (Cédula de Identidad)
  - Argentina (DNI)
  - Brasil (CPF)
  - Chile (RUT)
  - México (CURP)
  - Colombia (Cédula de Ciudadanía)
  - Perú (DNI)
  - España (DNI/NIE)
  - Patrón genérico personalizable
- 📞 Detección de números de teléfono
- 📁 Identificación de archivos sensibles (.pdf, .doc, .xls, .sql, .bak, etc.)
- 🔄 Crawling recursivo con control de profundidad
- 🚀 Multi-threading para escaneos rápidos
- 💻 Modo interactivo y CLI
- 📊 Reportes en JSON y TXT

---

## Tipos de cambios

- `Agregado` para nuevas funcionalidades
- `Cambiado` para cambios en funcionalidades existentes
- `Obsoleto` para funcionalidades que serán eliminadas próximamente
- `Eliminado` para funcionalidades eliminadas
- `Arreglado` para corrección de errores
- `Seguridad` para vulnerabilidades
